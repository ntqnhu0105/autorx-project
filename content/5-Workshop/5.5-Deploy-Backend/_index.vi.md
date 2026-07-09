---
title : "Triển khai Backend API"
date : 2026-07-09
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ triển khai **Backend API** của hệ thống AutoRx bằng **AWS Lambda** và **Amazon API Gateway**.

Backend xử lý toàn bộ các API request từ **Web Dashboard**, bao gồm truy vấn dữ liệu phương tiện, lịch sử telemetry, lịch bảo dưỡng và quản lý người dùng — tất cả dữ liệu được lưu trong **Amazon DynamoDB**.

#### Kiến trúc

Luồng request hoạt động như sau:

1. Người dùng truy cập Web Dashboard được host trên **Amazon S3 + CloudFront**
2. Dashboard xác thực qua **Amazon Cognito** và nhận **JWT token**
3. Các API call được gửi đến **Amazon API Gateway** kèm JWT trong header `Authorization`
4. API Gateway xác thực token qua **Cognito Authorizer** và gọi **AWS Lambda (API Handler)** tương ứng
5. Lambda function đọc/ghi dữ liệu trong **Amazon DynamoDB** và trả về JSON response

![Backend Architecture](/images/5-Workshop/5.5-BackendAPI/backend-overview.png)

#### Nội dung

- [5.5.1 - Triển khai Lambda API Handler](5.5.1-deploy-backend/)
- [5.5.2 - Cấu hình Environment Variables](5.5.2-env/)
- [5.5.3 - Thiết lập REST API với API Gateway](5.5.3-rest-api/)
- [5.5.4 - Kiểm tra CRUD với DynamoDB](5.5.4-crud-dynamodb/)
