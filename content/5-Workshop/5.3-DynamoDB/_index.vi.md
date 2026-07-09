---
title : "Triển khai Amazon DynamoDB"
date : 2026-07-09
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

#### Triển khai Data Layer

Trong phần này, chúng ta sẽ triển khai tầng lưu trữ dữ liệu của hệ thống AutoRx bằng **Amazon DynamoDB**.

Hệ thống sử dụng Amazon DynamoDB làm cơ sở dữ liệu NoSQL để lưu trữ dữ liệu phương tiện, dữ liệu Telemetry, lịch bảo dưỡng và thông tin người dùng. Thiết kế này giúp hệ thống có khả năng mở rộng cao, đáp ứng tốt các ứng dụng xử lý dữ liệu thời gian thực trên kiến trúc Serverless.

Workshop sẽ tạo bốn bảng DynamoDB tương ứng với từng nhóm dữ liệu của hệ thống:

- **SmartVehicle_Vehicles**: Lưu thông tin phương tiện.
- **SmartVehicle_Telemetry**: Lưu dữ liệu Telemetry theo thời gian.
- **SmartVehicle_Maintenance**: Lưu lịch bảo dưỡng của từng phương tiện.
- **SmartVehicle_Users**: Lưu thông tin người dùng.

![architecture](/images/5-Workshop/5.3-DynamoDB/dynamoDB-overview.jpg)

#### Nội dung

- [Tạo bảng SmartVehicle_Vehicles](5.3.1-Vehicles/)
- [Tạo Global Secondary Indexes](5.3.2-Indexes/)
- [Tạo bảng SmartVehicle_Telemetry](5.3.3-Telemetry/)
- [Tạo bảng SmartVehicle_Maintenance](5.3.4-Maintenance/)
- [Tạo bảng SmartVehicle_Users](5.3.5-Users/)
- [Kiểm tra Data Layer](5.3.6-Verify/)