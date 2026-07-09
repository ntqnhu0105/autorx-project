---
title: "Bản đề xuất"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# AutoRx: Hệ thống Giám sát & Nhắc lịch Bảo dưỡng Xe thông minh
## Nền tảng Thu thập Telemetry Thời gian thực và Chẩn đoán Sự cố bằng Trí tuệ Nhân tạo trên AWS Cloud

### 1. Tóm tắt điều hành
AutoRx là một nền tảng IoT và đám mây tiên tiến được thiết kế để giải quyết bài toán giám sát sức khỏe xe và nhắc lịch bảo dưỡng chủ động. Hệ thống thực hiện thu thập dữ liệu telemetry thời gian thực (bao gồm vận tốc, nhiệt độ động cơ, điện áp bình ắc quy và số ODO tích lũy) truyền qua giao thức MQTT bảo mật. Tận dụng kiến trúc AWS Serverless và IoT Core, dữ liệu cảm biến được lọc và ghi nhận tự động vào cơ sở dữ liệu NoSQL (DynamoDB). Hệ thống cung cấp giao diện Web Dashboard (Next.js) trực quan, cơ chế cảnh báo tự động qua Amazon CloudWatch/SNS và tích hợp trí tuệ nhân tạo Google Gemini AI để chẩn đoán và tư vấn cách xử lý sự cố tức thời cho tài xế khi xe gặp lỗi nghiêm trọng.

### 2. Tuyên bố vấn đề
*Vấn đề hiện tại:*
Công tác bảo dưỡng xe hiện nay đa phần mang tính thụ động. Tài xế thường không nhận biết được các dấu hiệu hư hỏng âm thầm (như động cơ quá nhiệt nhẹ hoặc sụt điện áp bình) cho đến khi xe hỏng hoàn toàn giữa đường. Việc theo dõi số ODO thủ công để thay nhớt, thay lọc gió hoặc kiểm tra phanh cũng rất dễ bị quên hoặc nhầm lẫn. Các thiết bị đọc lỗi OBD-II truyền thống thì phức tạp và thiếu khả năng đồng bộ đám mây cũng như không hỗ trợ đưa ra các chỉ dẫn xử lý dễ hiểu bằng ngôn ngữ tự nhiên cho người dùng.

*Giải pháp:*
AutoRx mang đến một kiến trúc đám mây toàn trình:
* **Thu thập dữ liệu IoT:** AWS IoT Core tiếp nhận các gói telemetry MQTT từ bộ giả lập cảm biến trên xe.
* **Chống mất mát dữ liệu:** Amazon SQS đóng vai trò hàng đợi trung chuyển, giảm tải và chống nghẽn hệ thống.
* **Xử lý phi máy chủ:** AWS Lambda bóc tách dữ liệu và cập nhật trạng thái xe vào Amazon DynamoDB.
* **Phân phối giao diện:** Amazon S3 và CloudFront CDN tối ưu hóa tải trang Web Dashboard Next.js.
* **Xác thực định danh:** Amazon Cognito quản lý tài khoản người dùng và phân quyền thông qua JWT Token.
* **Chẩn đoán thông minh:** Google Gemini AI tư vấn xử lý sự cố tại chỗ dựa trên các thông số lỗi thực tế của xe.
* **Giám sát & Cảnh báo:** Amazon CloudWatch Logs/Alarms và Amazon SNS tự động gửi Email cảnh báo khẩn cấp.

*Lợi ích và hoàn vốn đầu tư (ROI):*
Hệ thống giúp giảm thiểu tối đa các rủi ro hư hỏng nặng nhờ cảnh báo sớm (ví dụ động cơ vượt ngưỡng 105°C). Lịch bảo dưỡng tự động giúp xe luôn vận hành trong tình trạng tốt nhất. Nhờ kiến trúc Serverless tối ưu, chi phí hạ tầng AWS chỉ tiêu tốn ước tính khoảng 0.99 USD/tháng cho quy mô thử nghiệm, mang lại hiệu quả đầu tư vượt trội so với các giải pháp phần cứng chuyên dụng đắt đỏ.

### 3. Kiến trúc giải pháp
Nền tảng ứng dụng kiến trúc AWS Serverless toàn phần để đảm bảo tính sẵn sàng cao và khả năng mở rộng tối đa.

![Sơ đồ kiến trúc hệ thống](/images/2-Proposal/architecture.jpg)

*Các dịch vụ AWS sử dụng:*
* **AWS IoT Core:** Đăng ký thiết bị và tiếp nhận bản tin MQTT bảo mật qua chứng chỉ X.509.
* **AWS IoT Rule Engine:** Lọc dữ liệu thô và chuyển tiếp vào hàng đợi SQS.
* **Amazon SQS (Standard Queue):** Hàng đợi đệm để giảm tải lưu lượng tin nhắn lớn gửi từ xe.
* **AWS Lambda:** Chạy các hàm xử lý dữ liệu (Telemetry Processor) và API xử lý nghiệp vụ (API Handler).
* **Amazon DynamoDB:** Cơ sở dữ liệu NoSQL lưu trữ hồ sơ xe, log hành trình cảm biến và lịch bảo dưỡng.
* **Amazon S3 & CloudFront:** Lưu trữ mã nguồn web tĩnh và tối ưu hóa tốc độ tải trang qua CDN.
* **Amazon Cognito:** Quản lý đăng ký tài khoản và phân quyền truy cập thông qua mã JWT.
* **Amazon API Gateway:** Cung cấp các REST API bảo mật cho ứng dụng web.
* **Amazon CloudWatch:** Ghi log tập trung, thu thập metrics hiệu năng và tạo cảnh báo lỗi.
* **Amazon SNS:** Phát các thông báo Email tự động cho người dùng khi có sự cố.

### 4. Triển khai kỹ thuật
*Các giai đoạn triển khai:*
1. **Phân tích & Thiết kế:** Thiết kế cấu trúc các bảng dữ liệu (Prisma/DynamoDB), vẽ sơ đồ luồng dữ liệu và kiến trúc hệ thống (Tuần 1 - 4).
2. **Thiết lập Pipeline Telemetry:** Cấu hình AWS IoT Core, hàng đợi SQS, viết Lambda Telemetry Processor và tạo DynamoDB (Tuần 5 - 8).
3. **Phát triển API & Web App:** Phát triển giao diện Web Dashboard Next.js, cấu hình S3/CloudFront, Cognito và Lambda API Handler (Tuần 9 - 11).
4. **Tích hợp AI & Kiểm thử:** Liên kết API Gemini, thiết lập CloudWatch/SNS Alarm và kiểm thử toàn trình (E2E Test) (Tuần 12).

*Yêu cầu kỹ thuật:*
* **Bộ giả lập cảm biến:** Chạy kịch bản giả lập các thông số động cơ gửi qua MQTT kèm chứng chỉ X.509 xác thực.
* **Backend:** Lập trình Node.js trên AWS Lambda để giao tiếp với DynamoDB thông qua AWS SDK.
* **Frontend:** Giao diện Next.js hiển thị biểu đồ thời gian thực, đồng hồ kim (Gauge) trực quan.
* **Bảo mật API:** API Gateway tích hợp Cognito Authorizer để kiểm tra chữ ký số JWT gửi kèm header.

### 5. Lộ trình & Mốc triển khai
* **Tuần 1 - 4:** Hoàn thành khảo sát nghiệp vụ, thiết kế ERD cơ sở dữ liệu và sơ đồ kiến trúc AWS.
* **Tuần 5 - 8:** Triển khai luồng nhận dữ liệu IoT Core, cấu hình SQS, Lambda và kiểm tra ghi nhận vào DynamoDB.
* **Tuần 9 - 11:** Hoàn thiện giao diện Web Dashboard, tích hợp xác thực Cognito và kết nối API chẩn đoán Gemini AI.
* **Tuần 12 (Kiểm thử & Báo cáo):** Thiết lập hệ thống cảnh báo CloudWatch/SNS, chạy thử nghiệm toàn bộ kịch bản lỗi, viết và nộp báo cáo.

### 6. Ước tính ngân sách
Bảng dự chi chi phí hạ tầng AWS hàng tháng ước tính qua AWS Pricing Calculator:

* **AWS IoT Core:** 0.08 USD/tháng (giả lập 5 thiết bị gửi tin nhắn liên tục)
* **Amazon SQS:** 0.00 USD/tháng (nằm trong Free Tier)
* **AWS Lambda:** 0.00 USD/tháng (nằm trong hạn mức 1 triệu request miễn phí)
* **Amazon DynamoDB:** 0.25 USD/tháng (lưu trữ và truy vấn thông số cơ bản)
* **Amazon S3:** 0.15 USD/tháng (lưu trữ mã nguồn web dashboard)
* **Amazon CloudFront:** 0.10 USD/tháng (băng thông CDN truyền tải giao diện)
* **Amazon Cognito:** 0.00 USD/tháng (miễn phí đến 50.000 người dùng hoạt động)
* **Amazon API Gateway:** 0.01 USD/tháng (đáp ứng các truy vấn đọc dữ liệu)
* **Amazon CloudWatch & SNS:** 0.40 USD/tháng (giám sát log và gửi email cảnh báo)

**Tổng cộng chi phí ước tính:** 0.99 USD / tháng (Khoảng 11.88 USD / năm)

### 7. Đánh giá rủi ro
* **Mất kết nối mạng cảm biến:** Nếu thiết bị biên mất mạng, dữ liệu telemetry không thể gửi về đám mây. *Giải pháp giảm thiểu:* Bộ giả lập lưu dữ liệu tạm thời vào cache cục bộ và gửi bù khi kết nối trở lại.
* **Hết hạn Token Cognito:** Người dùng bị logout đột ngột khi đang giám sát. *Giải pháp giảm thiểu:* Thiết lập cơ chế tự động làm mới Token (Refresh Token) trên giao diện Next.js client.
* **Chi phí vượt tầm kiểm soát:** Quên tắt tài nguyên hoặc bị tấn công làm cạn kiệt tài khoản. *Giải pháp giảm thiểu:* Cài đặt cảnh báo ngân sách AWS Budgets, tự động tắt tài nguyên nếu chi phí vượt quá 2.00 USD/tháng.

### 8. Kết quả kỳ vọng
* **Giám sát thời gian thực:** Giao diện dashboard cập nhật liên tục thông số với độ trễ dưới 2 giây.
* **Chẩn đoán thông minh bằng AI:** Tài xế nhận được hướng dẫn xử lý sự cố khẩn cấp viết bằng ngôn ngữ tự nhiên từ Gemini AI khi xe báo lỗi đỏ.
* **Bảo dưỡng chủ động:** Hệ thống tự động tính toán chính xác chu kỳ và hiển thị thanh tiến độ trực quan của các hạng mục thay nhớt, kiểm tra phanh dựa trên số ODO.