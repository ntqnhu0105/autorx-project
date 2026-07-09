---
title : "Triển khai AWS Lambda Telemetry Processor"
date : 2026-07-09
weight : 4
chapter : false
pre : " <b> 5.4.4. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ triển khai **AWS Lambda Telemetry Processor** để tự động xử lý các bản tin Telemetry được gửi từ **Amazon SQS**.

AWS Lambda sẽ đọc từng bản tin trong Queue, phân tích dữ liệu Telemetry, cập nhật trạng thái phương tiện và lưu kết quả vào các bảng **Amazon DynamoDB**.

![Lambda Architecture](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/lambda-architecture.jpg)

---

### Tạo IAM Role cho Lambda

Truy cập **IAM**.

Chọn **Roles** → **Create role**.

Chọn:

```
AWS Service
```

Use case:

```
Lambda
```

Đặt tên Role:

```
AutoRxTelemetryLambdaRole
```

![create-role](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/create-role.jpg)

---

### Gán quyền cho Lambda

Sau khi tạo Role, gắn các Permission Policy:

```
AWSLambdaBasicExecutionRole
```

```
AmazonDynamoDBFullAccess
```

Nếu hệ thống sử dụng Amazon SNS để gửi Email cảnh báo, gắn thêm:

```
AmazonSNSFullAccess
```

![attach-policy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/attach-policy.jpg)

---

### Tạo Lambda Function

Truy cập **AWS Lambda**.

Chọn **Create function**.

Nhập các thông tin:

| Property | Value |
|----------|-------|
| Function name | AutoRxTelemetryProcessor |
| Runtime | Node.js 22.x |
| Architecture | x86_64 |
| Execution Role | AutoRxTelemetryLambdaRole |

Sau đó chọn **Create function**.

![create-function](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/create-function.jpg)

---

### Cấu hình Amazon SQS Trigger

Trong Lambda Function vừa tạo.

Chọn:

```
Add Trigger
```

Source:

```
Amazon SQS
```

Queue:

```
AutoRxTelemetryQueue
```

Batch size:

```
10
```

Sau đó chọn **Add**.

![add-trigger](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/add-trigger.jpg)

---

### Cấu hình Environment Variables

Chọn:

```
Configuration

↓

Environment variables
```

Thêm các biến môi trường:

| Key | Value |
|------|-------|
| REGION | ap-southeast-1 |
| VEHICLE_TABLE | SmartVehicle_Vehicles |
| TELEMETRY_TABLE | SmartVehicle_Telemetry |
| MAINTENANCE_TABLE | SmartVehicle_Maintenance |

Sau đó chọn **Save**.

![environment](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/env.jpg)

---

### Triển khai Source Code

Trong mục **Code** của Lambda Function.

Chọn:

```
Upload from
↓
.zip file
```

Chọn file source code Telemetry Processor và nhấn **Deploy**.

![deploy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/deploy_lambda.jpg)

---

### Kiểm tra AWS Lambda

Quay lại **Vehicle Simulator** và gửi một bản tin Telemetry mới.

Sau vài giây, truy cập:

```
AWS Lambda

↓

Monitor

↓

View CloudWatch Logs
```

Nếu Lambda xử lý thành công, CloudWatch Logs sẽ hiển thị thông tin xử lý Telemetry.

![cloudwatch](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/cloudwatch.jpg)

---

Tiếp tục truy cập **Amazon DynamoDB** và kiểm tra các bảng:

- SmartVehicle_Telemetry
- SmartVehicle_Vehicles
- SmartVehicle_Maintenance

Nếu dữ liệu đã được cập nhật, AWS Lambda đã xử lý Telemetry thành công.

![dynamodb](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/dynamodb1.jpg)
![dynamodb](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/dynamodb2.jpg)
![dynamodb](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/dynamodb3.jpg)