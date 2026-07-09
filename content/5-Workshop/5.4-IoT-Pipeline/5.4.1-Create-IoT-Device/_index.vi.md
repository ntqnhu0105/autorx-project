---
title : "Tạo thiết bị IoT"
date : 2026-07-09
weight : 1
chapter : false
pre : " <b> 5.4.1. </b> "
---

### Tạo thiết bị IoT

Trong hệ thống AutoRx, **Vehicle Simulator** đóng vai trò mô phỏng phương tiện và gửi dữ liệu Telemetry như tốc độ, nhiệt độ động cơ, điện áp ắc quy và số kilomet đã di chuyển lên AWS Cloud.

Để một thiết bị có thể kết nối an toàn với **AWS IoT Core**, trước tiên cần tạo một **IoT Thing**. Sau đó AWS sẽ cấp **X.509 Certificate** để xác thực thiết bị khi gửi dữ liệu thông qua giao thức MQTT.

Trong phần này, chúng ta sẽ:

- Tạo một AWS IoT Thing.
- Sinh X.509 Certificate.
- Tạo IoT Policy.
- Gắn Certificate và Policy vào Thing.
- Chuẩn bị các thông tin cần thiết để Vehicle Simulator kết nối đến AWS IoT Core.

---

### Kiến trúc kết nối thiết bị

Trước khi dữ liệu Telemetry được gửi vào hệ thống, **Vehicle Simulator** cần thiết lập kết nối an toàn với **AWS IoT Core**.

AWS IoT Core sử dụng **X.509 Certificate** để xác thực danh tính của thiết bị. Sau khi xác thực thành công, Vehicle Simulator sẽ sử dụng giao thức **MQTT** để publish dữ liệu Telemetry lên các MQTT Topic đã được cấu hình.

Hình dưới đây mô tả quá trình kết nối giữa Vehicle Simulator và AWS IoT Core.

![IoT Device Connection](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/iot-device-connection.jpg)

## Tạo AWS IoT Thing

Đăng nhập vào **AWS Management Console**, tìm kiếm dịch vụ **AWS IoT Core**.

![iot-core](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/iot-core.jpg)

Từ menu bên trái, chọn:

```
Manage
    └── All devices
            └── Things
```

Sau đó chọn **Create things**.

![things](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/things.jpg)

Chọn **Create single thing** để tạo một thiết bị mới.

![single-thing](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/create-single.jpg)

Nhập thông tin:

| Thuộc tính | Giá trị |
|------------|----------|
| Thing name | AutoRx-Vehicle-01 |

Các cấu hình còn lại giữ mặc định.

Sau đó chọn **Next**.

![thing-name](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/thing-name.jpg)

---

## Tạo X.509 Certificate

AWS IoT Core sử dụng **X.509 Certificate** để xác thực thiết bị.

Tại màn hình Certificate, chọn:

```
Auto-generate a new certificate
```

Sau đó chọn **Next**.

![certificate](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/certificate.jpg)

AWS sẽ tự động tạo các file chứng thực cho thiết bị.

Tải xuống toàn bộ các file sau:

- Device certificate
- Public key file
- Private key file
- Amazon Root CA 1

Đây là các file bắt buộc để Vehicle Simulator có thể kết nối MQTT đến AWS IoT Core.

{{% notice warning %}}
Không thể tải lại **Private Key** sau khi đóng màn hình này. Hãy lưu trữ các file Certificate ở vị trí an toàn trước khi tiếp tục.
{{% /notice %}}

![download](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/download-certificate.jpg)

---

## Tạo IoT Policy

Sau khi tải Certificate, chọn **Create policy**.

![create-policy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/create-policy.jpg)

Nhập các thông tin sau:

| Thuộc tính | Giá trị |
|------------|----------|
| Policy name | AutoRx-IoT-Policy |

Thêm một Policy Statement với các giá trị:

| Thuộc tính | Giá trị |
|------------|----------|
| Effect | Allow |
| Action | iot:* |
| Resource ARN | * |

Sau đó chọn **Create**.

![policy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/policy.jpg)

> Trong môi trường Production, bạn nên giới hạn quyền truy cập theo Topic hoặc ARN cụ thể. Workshop này sử dụng quyền `iot:*` để đơn giản hóa quá trình thực hành.

---

## Gắn Policy vào Thing

Sau khi tạo Policy thành công, quay trở lại trình hướng dẫn.

Chọn Policy vừa tạo:

```
AutoRx-IoT-Policy
```

Sau đó chọn **Next**.

AWS sẽ tự động:

- Attach Certificate
- Attach Policy
- Attach Thing

Cuối cùng chọn **Done** để hoàn tất.

![attach](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/attach-policy.jpg)

---

## Kiểm tra kết quả

Quay lại:

```
Manage
    └── Things
```

Chọn **AutoRx-Vehicle-01**.

Kiểm tra:

- Thing đã được tạo thành công.
- Certificate ở trạng thái **Active**.
- Policy **AutoRx-IoT-Policy** đã được gắn.

![result](/images/5-Workshop/5.4-IoT-Pipeline/5.4.1-create-iot-device/result.jpg)

Sau khi hoàn thành, Vehicle Simulator đã có đầy đủ thông tin xác thực để kết nối đến AWS IoT Core thông qua giao thức MQTT. Trong phần tiếp theo, chúng ta sẽ cấu hình **AWS IoT Rule** để chuyển tiếp dữ liệu Telemetry sang Amazon SQS.

