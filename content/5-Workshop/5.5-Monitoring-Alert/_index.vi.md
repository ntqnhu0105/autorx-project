---
title: "Giám sát & Cảnh báo"
date: 2026-07-09
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

# Giám sát hệ thống & Gửi cảnh báo

Để đảm bảo hệ thống vận hành trơn tru và phát hiện kịp thời các sự cố nguy hiểm từ xe cộ, dự án đã tích hợp một luồng Giám sát & Cảnh báo hoàn toàn tự động dựa trên hệ sinh thái của AWS.

#### 1. Amazon CloudWatch Logs - Ghi chép lịch sử
Tất cả các dịch vụ (IoT Core, API Gateway, Lambda) đều được cấu hình đẩy log về **CloudWatch Logs**.
* Mọi sự kiện từ lúc xe kết nối gửi MQTT, đến việc Lambda thực thi hàm kiểm tra nhiệt độ đều được lưu trữ dạng text.
* Nhờ đó, người quản trị có thể dễ dàng truy vết (debug) khi có bản tin bị lỗi hoặc ứng dụng web không kết nối được tới cơ sở dữ liệu.

![Cloudwatch](/images/5-Workshop/5.5-Monitoring-alert/send-telemetry.jpg)

#### 2. CloudWatch Metrics & Alarms - Phát hiện bất thường
Dự án không chỉ lưu log mà còn đo lường (Metrics).
* **Metrics:** Hệ thống thiết lập các bộ đếm theo dõi tần suất gửi dữ liệu của xe và nhiệt độ động cơ hiện tại.
* **Alarms:** Một "báo động" (CloudWatch Alarm) được cài đặt để liên tục theo dõi các Metrics này. Khi phát hiện chỉ số nhiệt độ động cơ vượt quá 105°C (hoặc Lambda báo lỗi liên tục), Alarm sẽ lập tức chuyển từ trạng thái `OK` sang `ALARM`.

#### 3. Amazon SNS - Phân phối thông báo khẩn cấp
Khi CloudWatch Alarm kích hoạt trạng thái báo động, hoặc khi thuật toán Lambda phát hiện xe đã quá hạn bảo dưỡng, hệ thống cần một kênh liên lạc với chủ xe:
* **SNS Topic:** Một kênh giao tiếp (Topic) được tạo ra.
* **Tự động gửi Email/SMS:** Email của người dùng/quản trị viên được đăng ký vào Topic này. Ngay khi có lỗi, Amazon SNS sẽ tự động phát tán một Email chứa nội dung khẩn cấp (ví dụ: "CẢNH BÁO: Động cơ xe vượt mức nhiệt an toàn") đến người dùng.

![SNS](/images/5-Workshop/5.5-Monitoring-alert/sns.jpg)
