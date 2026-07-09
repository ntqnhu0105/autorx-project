---
title: "Luồng Telemetry (IoT Core & Lambda)"
date: 2026-07-09
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

# Thiết lập luồng dữ liệu Telemetry

#### 1. Cấu hình AWS IoT Core
* Tạo một **Thing** trong AWS IoT Core đại diện cho chiếc xe thông minh.

![thing](/images/5-Workshop/5.3-Telemetry-flow/thing-name.jpg)

* Tải xuống bộ **Certificates** (Public key, Private key, Device certificate) để cho phép thiết bị mô phỏng xác thực và kết nối an toàn với AWS IoT qua giao thức **MQTT**.

![cert](/images/5-Workshop/5.3-Telemetry-flow/download-certificate.jpg)

* Tạo và đính kèm **IoT Policy** vào Certificate để cấp quyền cho thiết bị được phép Publish và Subscribe vào các MQTT topics cụ thể (VD: `telemetry/vehicle/#`).

![policy](/images/5-Workshop/5.3-Telemetry-flow/result.jpg)

#### 2. Tạo AWS Lambda Function xử lý dữ liệu
* Truy cập dịch vụ AWS Lambda và tạo một hàm mới (Runtime: Node.js hoặc Python tùy chọn).

![function](/images/5-Workshop/5.3-Telemetry-flow/create-function.jpg)

* Hàm Lambda này sẽ chịu trách nhiệm: 
  - Phân tích cú pháp (parse) chuỗi JSON chứa dữ liệu gửi từ thiết bị.
  - Kiểm tra các chỉ số quan trọng (nhiệt độ động cơ, vận tốc).
  - Tính toán quãng đường để xác định xe đã đến hạn bảo dưỡng hay chưa.
* Gán **IAM Role** cho Lambda để cho phép ghi log vào CloudWatch và tương tác với DynamoDB ở các bước sau.

#### 3. Cấu hình IoT Rule (Message Routing)
* Trong AWS IoT Core, tạo một **Message Routing Rule** để tự động chuyển tiếp dữ liệu (message) nhận được từ MQTT Topic sang Lambda Function vừa tạo.
* Viết câu lệnh SQL truy vấn dữ liệu (VD: `SELECT * FROM 'telemetry/vehicle/+'`).

![sql](/images/5-Workshop/5.3-Telemetry-flow/sql.jpg)

* Cấu hình **Action** cho Rule là gọi (invoke) hàm Lambda đích, cho phép quy trình xử lý dữ liệu hoàn toàn tự động theo thời gian thực.

![action](/images/5-Workshop/5.3-Telemetry-flow/rule-actions.jpg)

