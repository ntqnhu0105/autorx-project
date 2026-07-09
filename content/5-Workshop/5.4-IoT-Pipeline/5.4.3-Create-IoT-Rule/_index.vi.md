---
title : "Tạo AWS IoT Rule"
date : 2026-07-09
weight : 3
chapter : false
pre : " <b> 5.4.3. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ tạo **AWS IoT Rule** để tự động chuyển tiếp dữ liệu Telemetry từ AWS IoT Core đến Amazon SQS.

![IoT Rule Architecture](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/iot-rule-architecture.jpg)

---

### Tạo AWS IoT Rule

- Truy cập **AWS IoT Core**.
- Chọn **Message routing** → **Rules**.
- Chọn **Create rule**.

![rules](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/rules.jpg)

---

### Cấu hình Rule

Nhập các thông tin:

| Property | Value |
|----------|-------|
| Rule name | AutoRxTelemetryRule |
| Description | Route telemetry messages to Amazon SQS |

![rule-info](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/rule-info.jpg)

---

### Cấu hình SQL Statement

Nhập câu lệnh:

```sql
SELECT *
FROM 'autorx/telemetry'
```

![sql](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/sql.jpg)

---

### Cấu hình Rule Action

Chọn:

```
Amazon SQS Queue
```

Queue:

```
AutoRxTelemetryQueue
```

Role:

```
AutoRxIoTRuleRole
```

![action](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/rule-actions.jpg)

---

### Hoàn tất

Kiểm tra lại thông tin và chọn **Create**.

![review](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/review.jpg)

---

### Kiểm tra AWS IoT Rule

Mở **MQTT Test Client** và Subscribe vào topic:

```
autorx/telemetry
```

Gửi một bản tin Telemetry từ Vehicle Simulator.

![mqtt-message](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/mqtt-message.jpg)

---

Tiếp tục truy cập **Amazon SQS**, chọn **AutoRxTelemetryQueue** và nhấn **Poll for messages**.

Nếu Queue nhận được bản tin Telemetry, AWS IoT Rule đã được cấu hình thành công.

![queue-message](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/queue-message.jpg)