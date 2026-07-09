---
title : "Thiết lập IoT Telemetry Pipeline"
date : 2026-07-09
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ xây dựng **Telemetry Pipeline** của hệ thống AutoRx bằng các dịch vụ AWS Serverless.

Pipeline tiếp nhận dữ liệu Telemetry từ **Vehicle Simulator** thông qua giao thức MQTT, sau đó sử dụng **AWS IoT Core** và **AWS IoT Rule** để định tuyến dữ liệu đến **Amazon SQS**. Cuối cùng, **AWS Lambda** sẽ xử lý dữ liệu, cập nhật trạng thái phương tiện và lưu kết quả vào **Amazon DynamoDB**.

Việc tách riêng các thành phần xử lý bằng Amazon SQS giúp hệ thống hoạt động ổn định hơn, giảm nguy cơ mất dữ liệu khi lưu lượng Telemetry tăng cao và cho phép AWS Lambda xử lý dữ liệu theo từng lô (batch).

![Telemetry Pipeline](/images/5-Workshop/5.4-IoT-Pipeline/pipeline-overview.jpg)

#### Nội dung

- [Tạo thiết bị IoT](5.4.1-Create-IoT-Device/)
- [Tạo Amazon SQS Queue](5.4.2-Create-SQS-Queue/)
- [Cấu hình AWS IoT Rule](5.4.3-Create-IoT-Rule/)
- [Triển khai AWS Lambda Telemetry Processor](5.4.4-Deploy-Telemetry-Lambda/)
- [Kiểm thử Telemetry Pipeline](5.4.5-Verify/)