---
title : "Dọn dẹp tài nguyên"
date : 2026-07-09
weight : 10
chapter : false
pre : " <b> 5.10. </b> "
---

#### Tổng quan

Chúc mừng bạn đã hoàn thành **workshop AutoRx**!

Trong phần cuối này, bạn sẽ xóa toàn bộ tài nguyên AWS đã tạo trong workshop để tránh phát sinh chi phí.

> **Lưu ý quan trọng**: Xóa tài nguyên theo đúng thứ tự bên dưới để tránh lỗi phụ thuộc.

---

### 1. Tắt và xóa CloudFront Distribution

1. Mở [CloudFront Console](https://console.aws.amazon.com/cloudfront)
2. Chọn distribution đã tạo cho AutoRx Web Dashboard
3. Nhấn **Disable** và chờ đến khi trạng thái chuyển sang `Disabled`
4. Nhấn **Delete** và xác nhận

---

### 2. Xóa S3 Bucket

1. Mở [S3 Console](https://console.aws.amazon.com/s3)
2. Chọn bucket chứa file tĩnh của Web Dashboard
3. Nhấn **Empty** → xác nhận, sau đó nhấn **Delete** → xác nhận

---

### 3. Xóa API Gateway

1. Mở [API Gateway Console](https://console.aws.amazon.com/apigateway)
2. Chọn `autorx-rest-api`
3. Nhấn **Actions** → **Delete API** và xác nhận

---

### 4. Xóa các Lambda Function

1. Mở [Lambda Console](https://console.aws.amazon.com/lambda)
2. Xóa các function sau:
   - `AutoRxAPIHandler`
   - `AutoRxTelemetryProcessor`

---

### 5. Xóa Cognito User Pool

1. Mở [Cognito Console](https://console.aws.amazon.com/cognito)
2. Chọn User Pool đã tạo cho AutoRx
3. Nhấn **Delete user pool** và xác nhận

---

### 6. Xóa SQS Queue

1. Mở [SQS Console](https://console.aws.amazon.com/sqs)
2. Chọn queue đã tạo cho telemetry pipeline
3. Nhấn **Delete** và xác nhận

---

### 7. Xóa tài nguyên AWS IoT

1. Mở [AWS IoT Core Console](https://console.aws.amazon.com/iot)
2. Vào **Manage → Things** → xóa thing của Vehicle Simulator
3. Vào **Security → Certificates** → xóa certificate liên quan
4. Vào **Security → Policies** → xóa IoT policy
5. Vào **Message Routing → Rules** → xóa IoT rule

---

### 8. Xóa các bảng DynamoDB

1. Mở [DynamoDB Console](https://console.aws.amazon.com/dynamodb)
2. Xóa các bảng sau:
   - `SmartVehicle_Vehicles`
   - `SmartVehicle_Telemetry`
   - `SmartVehicle_Maintenance`
   - `SmartVehicle_Users`

---

### 9. Xóa CloudWatch Logs và Alarms

1. Mở [CloudWatch Console](https://console.aws.amazon.com/cloudwatch)
2. Vào **Logs → Log groups**
3. Xóa log group `/aws/lambda/AutoRxAPIHandler` và `/aws/lambda/AutoRxTelemetryProcessor`
4. Vào **Alarms** → xóa các alarm đã tạo trong workshop

---

### 10. Xóa SNS Topic

1. Mở [SNS Console](https://console.aws.amazon.com/sns)
2. Chọn topic đã tạo cho AutoRx notifications
3. Nhấn **Delete** và xác nhận

---

Toàn bộ tài nguyên AWS đã được dọn dẹp. Bạn đã hoàn thành thành công workshop AutoRx.
