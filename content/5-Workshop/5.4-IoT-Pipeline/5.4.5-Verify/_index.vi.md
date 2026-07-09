---
title : "Kiểm tra Telemetry Pipeline"
date : 2026-07-09
weight : 5
chapter : false
pre : " <b> 5.4.5. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ kiểm tra toàn bộ **Telemetry Pipeline** sau khi hoàn thành cấu hình AWS IoT Core, Amazon SQS và AWS Lambda.

Mục tiêu là xác nhận dữ liệu Telemetry được truyền thành công từ Vehicle Simulator đến Amazon DynamoDB.

![verify-pipeline](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/verify-pipeline.jpg)

---

### Gửi dữ liệu Telemetry

Mở **Vehicle Simulator**.

Nhập các thông số mô phỏng và chọn:

```
Send Telemetry
```

Vehicle Simulator sẽ gửi dữ liệu Telemetry đến AWS IoT Core thông qua MQTT.

![send-telemetry](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/telemetry.jpg)

---

### Kiểm tra Amazon SQS

Truy cập:

```
Amazon SQS
↓
AutoRxTelemetryQueue
↓
Send and receive messages
```

Chọn:

```
Poll for messages
```

Nếu Queue nhận được bản tin mới, AWS IoT Rule đã chuyển tiếp dữ liệu thành công.

![queue-message](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/queue-message.jpg)

---

### Kiểm tra AWS Lambda

Truy cập:

```
AWS Lambda

↓

AutoRxTelemetryProcessor

↓

Monitor

↓

View CloudWatch Logs
```

Nếu Lambda được kích hoạt thành công, CloudWatch Logs sẽ hiển thị quá trình xử lý Telemetry.

![lambda-log](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/lambda-log.jpg)

---

### Kiểm tra Amazon DynamoDB

Truy cập:

```
Amazon DynamoDB
```

Mở bảng:

```
SmartVehicle_Telemetry
```

Chọn:

```
Explore table items
```

Nếu xuất hiện bản ghi Telemetry mới, dữ liệu đã được xử lý và lưu thành công.

![telemetry-table](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/telemetry-table.jpg)

---

### Kết quả

Sau khi hoàn thành các bước trên, Telemetry Pipeline đã hoạt động thành công.

Dữ liệu Telemetry được xử lý theo kiến trúc:

```
Vehicle Simulator
        │
HTTP POST
        ▼
Next.js API
        │
MQTT Publish
        ▼
AWS IoT Core
        │
AWS IoT Rule
        ▼
Amazon SQS
        │
AWS Lambda
        ▼
Amazon DynamoDB
```

Tiếp theo, chúng ta sẽ triển khai **Backend REST API** để Dashboard và các ứng dụng khác có thể truy cập dữ liệu từ hệ thống AutoRx.