---
title: "Luồng Quản lý (API Gateway & DynamoDB)"
date: 2026-07-09
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

# Luồng Quản lý dữ liệu với API Gateway và DynamoDB

#### 1. Tạo Amazon DynamoDB Table
* Truy cập Amazon DynamoDB và tạo một bảng mới (ví dụ: `VehicleData`).
* Cấu hình **Partition Key** (khóa chính) là `vehicleId` (Kiểu String) để dễ dàng truy vấn dữ liệu của từng xe cụ thể.
* Thiết lập các thuộc tính bổ sung nếu cần (ví dụ như `timestamp` làm Sort Key để lấy lịch sử theo thời gian).
* Lựa chọn chế độ tính phí (Capacity mode) là On-demand để tiết kiệm chi phí cho bài thực hành.

![table](/images/5-Workshop/5.4-Management-flow/table.jpg)


#### 2. Tạo REST API với API Gateway
* Truy cập Amazon API Gateway, chọn tạo mới một **REST API** (hoặc HTTP API).
* Thiết lập các **Resources** và **Methods** cần thiết. Ví dụ:
  - Tạo resource `/vehicle/{id}`.
  - Thêm phương thức `GET` để nhận request từ phía người dùng (Frontend) và truy vấn thông tin chi tiết của xe.
* Bật tính năng **CORS** (Cross-Origin Resource Sharing) để Frontend có thể gọi API mà không bị lỗi trình duyệt chặn.

![API](/images/5-Workshop/5.4-Management-flow/API.jpg)


#### 3. Kết nối API với Lambda (Integration)
* Tạo một **AWS Lambda function** khác chuyên xử lý logic và các request HTTP của API.
* Trong cấu hình Method của API Gateway, chọn **Integration type** là Lambda Function và trỏ đến hàm vừa tạo.
* Cấp quyền (IAM Policy) cho Lambda này được phép thực hiện các thao tác đọc (`dynamodb:GetItem`, `dynamodb:Query`) và ghi (`dynamodb:PutItem`) vào bảng DynamoDB.
* Triển khai (Deploy) API lên một Stage (ví dụ: `dev` hoặc `prod`) và lưu lại **Invoke URL** để cấu hình cho Frontend.

![API](/images/5-Workshop/5.4-Management-flow/Lambda-api.jpg)

