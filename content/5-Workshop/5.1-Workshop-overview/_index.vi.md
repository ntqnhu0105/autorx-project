---
title : "Giới thiệu"
date : 2026-07-08 
weight : 1
chapter : false
pre : " <b> 5.1. </b> "
---

#### Giới thiệu về AutoRx

AutoRx là hệ thống giám sát sức khỏe xe theo thời gian thực được xây dựng trên kiến trúc Serverless của AWS. Hệ thống thu thập dữ liệu Telemetry từ Vehicle Simulator thông qua giao thức MQTT, xử lý bằng AWS Lambda và lưu trữ trên Amazon DynamoDB. Người dùng có thể theo dõi trạng thái xe thông qua Web Dashboard, đồng thời nhận cảnh báo khi xe gặp sự cố hoặc đến thời điểm bảo dưỡng.

Hệ thống được xây dựng với mục tiêu:

- Thu thập dữ liệu Telemetry theo thời gian thực.
- Giám sát tình trạng hoạt động của phương tiện.
- Tự động nhắc lịch bảo dưỡng dựa trên số ODO.
- Cung cấp Dashboard trực quan để theo dõi xe.
- Gửi Email cảnh báo khi phát hiện bất thường.
- Áp dụng kiến trúc Serverless nhằm giảm chi phí vận hành và tăng khả năng mở rộng.

---

#### Tổng quan về workshop
Trong workshop này, chúng ta sẽ triển khai hoàn chỉnh hệ thống AutoRx trên AWS Cloud theo kiến trúc Serverless.
Hệ thống bao gồm hai luồng hoạt động chính:
1. **Telemetry Pipeline**:
  - Vehicle Simulator đóng vai trò mô phỏng thiết bị IoT trên xe và gửi dữ liệu Telemetry đến **AWS IoT Core** thông qua giao thức MQTT sử dụng chứng chỉ **X.509**.
  - Sau khi nhận dữ liệu, **AWS IoT Rule Engine** sẽ chuyển tiếp bản tin đến **Amazon SQS** để đảm bảo khả năng xử lý ổn định khi số lượng bản tin tăng cao.
  - Các bản tin trong hàng đợi sẽ được **AWS Lambda (Telemetry Processor)** xử lý và lưu vào **Amazon DynamoDB**. Đồng thời Lambda ghi log lên **Amazon CloudWatch** và kích hoạt **Amazon SNS** gửi Email cảnh báo khi phát hiện các thông số vượt ngưỡng cho phép.

2. **Web Dashboard**:
  - Người dùng truy cập Dashboard thông qua **Amazon CloudFront**, nội dung website được lưu trữ trên **Amazon S3**.
  - Để truy cập dữ liệu hệ thống, người dùng đăng nhập bằng **Amazon Cognito** và nhận JWT Token.
  - JWT sẽ được gửi kèm trong các yêu cầu đến **Amazon API Gateway**. Sau khi xác thực thành công, API Gateway sẽ gọi **AWS Lambda (API Handler)** để truy vấn dữ liệu trong **Amazon DynamoDB** và trả kết quả về Dashboard.

---

#### Kiến trúc hệ thống
Bên dưới là kiến trúc tổng thể của AutoRx

![overview](/images/5-Workshop/5.1-Workshop-overview/architect.jpg)

---

#### Các dịch vụ AWS sử dụng

Trong workshop này, chúng ta sẽ sử dụng các dịch vụ AWS sau:

- **AWS IoT Core** tiếp nhận dữ liệu Telemetry từ Vehicle Simulator thông qua giao thức MQTT.
- **AWS IoT Rule Engine** chuyển tiếp dữ liệu từ AWS IoT Core đến Amazon SQS.
- **Amazon SQS** làm hàng đợi trung gian giúp xử lý dữ liệu ổn định và tránh mất bản tin.
- **AWS Lambda** xử lý dữ liệu Telemetry và triển khai Backend API.
- **Amazon DynamoDB** lưu trữ dữ liệu Telemetry, trạng thái xe và lịch bảo dưỡng.
- **Amazon Cognito** quản lý người dùng và cung cấp JWT Token cho quá trình xác thực.
- **Amazon API Gateway** cung cấp REST API cho Web Dashboard.
- **Amazon S3** lưu trữ mã nguồn Web Dashboard.
- **Amazon CloudFront** phân phối Dashboard thông qua CDN nhằm cải thiện tốc độ truy cập.
- **Amazon CloudWatch** thu thập log, theo dõi hiệu năng và tạo cảnh báo.
- **Amazon SNS** gửi Email thông báo khi hệ thống phát hiện sự cố hoặc đến thời điểm bảo dưỡng.

---

#### Luồng hoạt động của hệ thống

Hệ thống hoạt động theo trình tự sau:

1. Vehicle Simulator gửi dữ liệu Telemetry đến AWS IoT Core bằng giao thức MQTT.
2. AWS IoT Rule Engine chuyển tiếp dữ liệu vào Amazon SQS.
3. AWS Lambda đọc dữ liệu từ SQS và cập nhật vào DynamoDB.
4. Lambda ghi log lên CloudWatch và phát hiện các điều kiện cảnh báo.
5. CloudWatch kích hoạt Amazon SNS gửi Email khi phát hiện lỗi.
6. Người dùng đăng nhập bằng Amazon Cognito để nhận JWT Token.
7. Dashboard gửi yêu cầu đến API Gateway kèm JWT.
8. API Gateway xác thực người dùng và gọi Lambda.
9. Lambda truy vấn DynamoDB và trả dữ liệu về Dashboard.