---
title: "Tổng quan về Workshop"
date: 2026-07-09
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

# Tổng quan về Workshop

#### 1. Bối cảnh và Vấn đề (Context & Problem)
Việc quản lý phương tiện giao thông (cá nhân, giao hàng, dịch vụ) theo cách thủ công hiện nay đang gặp nhiều hạn chế. Chủ xe thường xuyên quên lịch bảo dưỡng định kỳ, khó theo dõi sát sao tình trạng hao mòn của các bộ phận quan trọng, dẫn đến nguy cơ hư hỏng nặng giữa đường hoặc gây mất an toàn giao thông. Hơn nữa, việc không có cơ sở dữ liệu lưu trữ lịch sử vận hành cũng khiến các doanh nghiệp vận tải gặp khó khăn trong việc tối ưu hóa chi phí và tuổi thọ của xe.

Để giải quyết bài toán này, chúng ta cần một hệ thống có khả năng tự động hóa việc thu thập dữ liệu (telemetry) liên tục từ các phương tiện và xử lý thông minh để đưa ra cảnh báo kịp thời.

#### 2. Mục tiêu của Workshop (Workshop Objectives)
Thông qua workshop này, bạn sẽ tự tay xây dựng được một hệ thống **Smart Vehicle Management** hoàn chỉnh ứng dụng các dịch vụ đám mây (Cloud) trên nền tảng AWS. Sau khi hoàn thành, bạn sẽ nắm bắt được:
* Cách kết nối các thiết bị IoT với đám mây thông qua giao thức MQTT.
* Kỹ năng xử lý dữ liệu thời gian thực (Real-time data processing) bằng kiến trúc Serverless.
* Cách thiết kế cơ sở dữ liệu NoSQL tối ưu cho luồng dữ liệu xe cộ.
* Khả năng thiết lập hệ thống giám sát và tự động kích hoạt cảnh báo thông minh qua Email/SMS.

#### 3. Kiến trúc hệ thống (System Architecture)

![architect](/images/5-Workshop/5.1-Workshop-overview/architect.jpg)

Kiến trúc của dự án được thiết kế theo mô hình **Event-Driven** (Hướng sự kiện) và **Serverless**, chia làm hai luồng xử lý độc lập nhưng liên kết chặt chẽ với nhau:

* **Luồng Telemetry (Dành cho xe):** 
  - Các thiết bị gắn trên xe (hoặc Vehicle Simulator) sẽ liên tục thu thập dữ liệu (vận tốc, nhiệt độ, quãng đường) và gửi trực tiếp lên **AWS IoT Core** thông qua MQTT. 
  - **IoT Rule** sẽ đóng vai trò người điều phối, tự động lọc và chuyển tiếp các thông điệp quan trọng này sang **AWS Lambda** để phân tích ngưỡng an toàn.
  - Sau khi xử lý, dữ liệu sẽ được lưu trữ dài hạn vào **Amazon DynamoDB**.

* **Luồng Quản lý (Dành cho người dùng/Dashboard):** 
  - Người dùng hoặc chủ xe muốn tra cứu thông tin bảo dưỡng sẽ sử dụng ứng dụng web (Frontend).
  - Ứng dụng này gửi các truy vấn (HTTP Requests) tới **Amazon API Gateway**.
  - API Gateway định tuyến các truy vấn đến một **AWS Lambda** độc lập khác, chuyên trách việc đọc/ghi dữ liệu trên **DynamoDB** và trả kết quả về cho người dùng.

* **Hệ thống Giám sát & Cảnh báo (Monitoring & Alerting):**
  - Mọi hoạt động của hệ thống được ghi log lại trên **CloudWatch Logs** để thuận tiện cho việc gỡ lỗi (debug).
  - Khi phát hiện sự kiện bất thường (ví dụ: nhiệt độ động cơ vượt quá 105°C), **CloudWatch Alarm** sẽ lập tức bị kích hoạt và ra lệnh cho **Amazon SNS** gửi tin nhắn cảnh báo khẩn cấp đến thiết bị của chủ xe.

Trong các phần tiếp theo, chúng ta sẽ đi sâu vào thực hành cài đặt từng thành phần trên!

