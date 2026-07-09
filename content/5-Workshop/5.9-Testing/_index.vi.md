---
title : "Kiểm thử hệ thống"
date : 2026-07-09
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ thực hiện **kiểm thử end-to-end** hệ thống AutoRx để xác nhận tất cả các thành phần hoạt động đúng và liên thông với nhau.

Luồng dữ liệu đầy đủ được kiểm thử:

1. **Vehicle Simulator** gửi telemetry qua MQTT đến **AWS IoT Core**
2. **IoT Rule** định tuyến message đến **Amazon SQS**
3. **AWS Lambda (AutoRxTelemetryProcessor)** nhận message, xử lý và ghi vào **DynamoDB**
4. Nếu phát hiện bất thường, **Amazon SNS** gửi email cảnh báo
5. Người dùng đăng nhập qua **Web Dashboard** bằng **Amazon Cognito**
6. Dashboard gọi **Amazon API Gateway** với JWT token
7. **AWS Lambda (AutoRxAPIHandler)** truy vấn DynamoDB và trả về dữ liệu
8. Dashboard hiển thị trạng thái phương tiện, lịch sử telemetry và lịch bảo dưỡng

![Testing Overview](/images/5-Workshop/5.9-Testing/testing-overview.png)

---

## 5.9.1 — Chạy Vehicle Simulator

Vehicle Simulator gửi MQTT message đến AWS IoT Core để giả lập xe thực đang truyền dữ liệu cảm biến.

### Các bước

1. Vào thư mục project simulator
2. Đảm bảo chứng chỉ IoT nằm trong thư mục `certs/`
3. Chạy simulator:

```bash
node simulator.js
```

Simulator sẽ publish message đến topic `vehicle/telemetry` mỗi vài giây.

Output mong đợi trên console:

```
[AutoRx Simulator] Connected to AWS IoT Core
[AutoRx Simulator] Published: { vehicleId: "default-vehicle-001", speed: 65, engineTemp: 88, ... }
[AutoRx Simulator] Published: { vehicleId: "default-vehicle-001", speed: 72, engineTemp: 91, ... }
```

![run-simulator](/images/5-Workshop/5.9-Testing/run-simulator.png)

---

## 5.9.2 — Kiểm tra dữ liệu Telemetry trong DynamoDB

Xác nhận Lambda Telemetry Processor đang ghi dữ liệu vào DynamoDB.

### Các bước

1. Mở [DynamoDB Console](https://console.aws.amazon.com/dynamodb)
2. Vào **Tables** → `SmartVehicle_Telemetry`
3. Nhấn **Explore table items**
4. Xác nhận các bản ghi mới xuất hiện với `vehicleId` đúng và `timestamp` gần nhất

Cấu trúc item mong đợi:

```json
{
  "vehicleId": "default-vehicle-001",
  "timestamp": "2026-07-09T10:00:00.000Z",
  "speed": 65,
  "engineTemp": 88,
  "batteryVoltage": 12.4,
  "isAbnormal": false
}
```

![verify-dynamodb](/images/5-Workshop/5.9-Testing/verify-dynamodb.png)

---

## 5.9.3 — Xem Lambda Logs trong CloudWatch

Xác nhận cả hai Lambda function đang thực thi không có lỗi.

### Các bước

1. Mở [CloudWatch Console](https://console.aws.amazon.com/cloudwatch)
2. Vào **Logs → Log groups**
3. Mở `/aws/lambda/AutoRxTelemetryProcessor`
4. Nhấn vào log stream mới nhất
5. Xác nhận log entries hiển thị xử lý message thành công:

```
START RequestId: ...
Processing 3 records from SQS
Wrote telemetry for vehicleId: default-vehicle-001
END RequestId: ...
REPORT Duration: 245.32 ms  Billed Duration: 246 ms  Memory Size: 256 MB
```

6. Mở `/aws/lambda/AutoRxAPIHandler` và xác nhận log API call xuất hiện khi dashboard gửi request

![check-logs](/images/5-Workshop/5.9-Testing/check-logs.png)

---

## 5.9.4 — Kiểm thử Web Dashboard

Thực hiện kiểm thử toàn bộ luồng người dùng qua Web Dashboard.

### Các bước

1. Mở CloudFront URL trên trình duyệt:
   ```
   https://xxxxxxxxxxxx.cloudfront.net
   ```
2. Đăng nhập bằng test user từ Phần 5.6.4
3. Xác nhận dashboard tải và hiển thị:
   - Danh sách phương tiện với chỉ số trạng thái
   - Dữ liệu telemetry thời gian thực (cập nhật khi simulator đang chạy)
   - Lịch bảo dưỡng với các hạng mục sắp đến
4. Điều hướng giữa các trang để xác nhận tất cả API call thành công (không có lỗi 401 hoặc 500 trong console trình duyệt)

![test-dashboard](/images/5-Workshop/5.9-Testing/test-dashboard.png)

---

## 5.9.5 — Xác nhận thông báo Email

Xác nhận phát hiện bất thường kích hoạt email cảnh báo qua SNS.

### Kích hoạt bất thường

Sửa simulator để gửi giá trị ngoài ngưỡng, hoặc chèn thủ công item với `isAbnormal: true` vào DynamoDB:

```bash
aws dynamodb put-item \
  --table-name SmartVehicle_Telemetry \
  --item '{
    "vehicleId": {"S": "default-vehicle-001"},
    "timestamp": {"S": "2026-07-09T12:00:00.000Z"},
    "engineTemp": {"N": "120"},
    "isAbnormal": {"BOOL": true}
  }' \
  --region ap-southeast-1
```

### Kiểm tra

- Kiểm tra hộp thư để nhận email cảnh báo từ `autorx-alerts`
- Email sẽ bao gồm vehicle ID và chi tiết bất thường

![verify-notifications](/images/5-Workshop/5.9-Testing/verify-notifications.png)

---

### Kết quả

Hệ thống AutoRx đã vượt qua kiểm thử end-to-end. Dữ liệu chạy đúng từ Vehicle Simulator qua IoT Core, SQS, Lambda, DynamoDB và hiển thị chính xác trên Web Dashboard với cảnh báo qua SNS.
