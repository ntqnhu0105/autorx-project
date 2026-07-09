---
title : "Tạo Amazon SQS Queue"
date : 2026-07-09
weight : 2
chapter : false
pre : " <b> 5.4.2. </b> "
---

#### Tổng quan

Trong phần này, chúng ta sẽ tạo một **Amazon SQS Queue** để lưu trữ tạm thời các bản tin Telemetry trước khi chúng được xử lý bởi **AWS Lambda**.

Việc sử dụng Amazon SQS giúp tách biệt quá trình tiếp nhận dữ liệu và xử lý nghiệp vụ. Khi lưu lượng Telemetry tăng cao, các bản tin sẽ được lưu vào hàng đợi thay vì gửi trực tiếp đến Lambda, giúp hệ thống ổn định hơn và hạn chế mất dữ liệu.

#### Nội dung

- [Tạo Amazon SQS Queue](5.4.2.1-create-queue/)
- [Cấu hình Queue](5.4.2.2-configure-queue/)
- [Kiểm tra Queue](5.4.2.3-verify/)