---
title: "Workshop"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# AutoRx - Hệ thống Giám sát và Nhắc lịch Bảo dưỡng Xe Thông minh trên AWS

#### Tổng quan

AutoRx là hệ thống giám sát sức khỏe xe theo thời gian thực được xây dựng trên kiến trúc **Serverless** của AWS. Hệ thống thu thập dữ liệu Telemetry từ Vehicle Simulator thông qua giao thức MQTT, xử lý bằng AWS Lambda, lưu trữ trên Amazon DynamoDB và hiển thị trực quan trên Web Dashboard.

Bên cạnh đó, hệ thống còn tích hợp các dịch vụ AWS nhằm đảm bảo khả năng mở rộng, bảo mật và giám sát:

- **AWS IoT Core** tiếp nhận dữ liệu Telemetry từ thiết bị.
- **Amazon SQS** đóng vai trò hàng đợi giúp xử lý dữ liệu ổn định.
- **AWS Lambda** xử lý dữ liệu và triển khai Backend API.
- **Amazon DynamoDB** lưu trữ dữ liệu xe và lịch bảo dưỡng.
- **Amazon Cognito** xác thực và quản lý người dùng.
- **Amazon API Gateway** cung cấp RESTful API cho Dashboard.
- **Amazon S3** và **CloudFront** triển khai Web Dashboard.
- **Amazon CloudWatch** theo dõi log và hiệu năng hệ thống.
- **Amazon SNS** gửi Email cảnh báo khi phát hiện sự cố.

#### Nội dung

1. [Tổng quan Workshop](5.1-Workshop-overview/)
2. [Chuẩn bị](5.2-Prerequisite/)
3. [Triển khai Amazon DynamoDB](5.3-DynamoDB/)
4. [Xây dựng IoT Telemetry Pipeline](5.4-IoT-Pipeline/)
5. [Triển khai Backend API](5.5-Backend-API/)
6. [Thiết lập Authentication](5.6-Authentication/)
7. [Triển khai Web Dashboard](5.7-Web-Dashboard/)
8. [Thiết lập Monitoring và Notification](5.8-Monitoring/)
9. [Kiểm thử hệ thống](5.9-Testing/)
10. [Dọn dẹp tài nguyên](5.10-Cleanup/)

