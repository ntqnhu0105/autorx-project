---
title: "Blog 2"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# [Data Caching Across Microservices trong kiến trúc Serverless](https://aws.amazon.com/blogs/architecture/data-caching-across-microservices-in-a-serverless-architecture/)

Các tổ chức đang dần chuyển đổi từ kiến trúc nguyên khối (Monolithic) sang **Microservices** để tăng khả năng mở rộng, phát triển nhanh hơn và triển khai các tính năng mới một cách độc lập.

Tuy nhiên, khi chia hệ thống thành nhiều microservice, một vấn đề mới xuất hiện: mỗi service thường phải lấy dữ liệu từ nhiều nguồn khác nhau như cơ sở dữ liệu, hệ thống legacy hoặc các dịch vụ dùng chung. Điều này khiến mỗi request phải thực hiện nhiều lời gọi đến backend, làm tăng độ trễ và tạo áp lực lớn lên hệ thống nguồn.

Để giải quyết bài toán đó, AWS đề xuất sử dụng Data Cache đặt gần tầng Microservices nhằm giảm số lần truy cập trực tiếp đến backend, từ đó cải thiện hiệu năng và giảm độ trễ.

---

## Tại sao cần Data Cache?

Cache là một lớp lưu trữ tốc độ cao dùng để lưu tạm những dữ liệu được truy cập thường xuyên.

Thay vì mỗi request đều truy vấn trực tiếp đến database hoặc hệ thống nguồn, microservice sẽ ưu tiên đọc dữ liệu từ Cache. Vì Cache thường được lưu trong bộ nhớ (memory), thời gian truy xuất nhanh hơn rất nhiều so với truy vấn trực tiếp đến backend.

Việc sử dụng Cache mang lại nhiều lợi ích:

- Giảm độ trễ khi xử lý request.
- Giảm số lần truy cập đến database hoặc hệ thống nguồn.
- Giảm tải cho backend khi lưu lượng truy cập tăng cao.
- Cải thiện khả năng mở rộng của toàn hệ thống.

Tùy theo đặc điểm dữ liệu và yêu cầu của ứng dụng, AWS giới thiệu hai chiến lược triển khai Cache khác nhau.

## Use case 1 – Cache dữ liệu theo nhu cầu (Cache-Aside Pattern)

![Blog2](/images/3-BlogsTranslated/Blog2_1.jpg)

> *Hình 1. Giảm độ trễ bằng cách lưu Cache các dữ liệu thường xuyên được truy cập.*

Trong kịch bản đầu tiên, AWS áp dụng **Cache-Aside Pattern**, hay còn gọi là **Lazy Loading**.

Ý tưởng của mô hình này rất đơn giản: dữ liệu chỉ được lưu vào Cache khi có người dùng yêu cầu.

Quy trình hoạt động gồm các bước:

1. Billing Service nhận request từ người dùng.
2. Service kiểm tra Cache bằng Object Key.
3. Nếu Cache đã có dữ liệu (Cache Hit), kết quả được trả về ngay.
4. Nếu Cache chưa có (Cache Miss), Billing Service truy vấn đến System of Record để lấy dữ liệu.
5. Dữ liệu sau khi lấy về được lưu vào Cache với một khoảng thời gian sống (TTL).
6. Những request tiếp theo sẽ đọc trực tiếp từ Cache thay vì truy cập backend.

Nhờ vậy, chỉ những dữ liệu thực sự được sử dụng mới được lưu trong Cache, giúp giảm đáng kể số lần truy vấn đến hệ thống nguồn.

---

## AWS triển khai Use case 1 như thế nào?

![Blog2](/images/3-BlogsTranslated/Blog2_2.jpg)

> *Hình 2. Các dịch vụ AWS được sử dụng để triển khai Cache-Aside.*

Để hiện thực mô hình trên trong môi trường Serverless, AWS sử dụng các dịch vụ sau:

- **Amazon API Gateway** tiếp nhận request từ client.
AWS Lambda triển khai các Microservices như Billing, Payment và Profile.
- **Amazon ElastiCache** đóng vai trò lớp Cache lưu dữ liệu trong bộ nhớ.
- **Amazon EventBridge** truyền các sự kiện giữa các microservice.
- **Cache Manager** chịu trách nhiệm cập nhật hoặc xóa dữ liệu trong Cache khi cần.

Khi dữ liệu thay đổi (ví dụ người dùng thanh toán thành công), Payment Service sẽ phát sinh một sự kiện gửi đến EventBridge. Cache Manager nhận sự kiện này để xóa hoặc cập nhật dữ liệu tương ứng trong Cache, giúp tránh việc người dùng đọc phải dữ liệu cũ.

---

## Hạn chế của Cache-Aside Pattern

Mặc dù Cache-Aside giúp giảm đáng kể số lần truy cập backend, mô hình này vẫn tồn tại một hạn chế.

Nếu dữ liệu trong hệ thống nguồn vừa được cập nhật nhưng bản sao trong Cache vẫn chưa hết thời gian sống (TTL), người dùng vẫn có thể nhận được dữ liệu cũ.

Đối với các hệ thống như thanh toán, tài khoản ngân hàng hoặc quản lý khách hàng, việc sử dụng dữ liệu cũ có thể gây ảnh hưởng đến nghiệp vụ.

Đó là lý do AWS giới thiệu chiến lược thứ hai.

---

## Use case 2 – Chủ động đồng bộ Cache bằng Event

![Blog2](/images/3-BlogsTranslated/Blog2_3.jpg)

> *Hình 3. Chủ động nạp dữ liệu vào Cache và đồng bộ bằng sự kiện.*

Khác với Cache-Aside, mô hình này không đợi request đầu tiên mới tạo Cache.

Ngay từ đầu, dữ liệu đã được nạp vào Cache thông qua các tiến trình tự động như **Initial Load** hoặc **Change Data Capture (CDC)**.

Khi dữ liệu trong hệ thống nguồn thay đổi:

1. Thay đổi được ghi nhận thông qua CDC Pipeline.
2. Một sự kiện được gửi đến Event Store.
3. Cache Manager nhận sự kiện.
4. Cache được cập nhật hoặc xóa ngay lập tức.
5. Các Microservices luôn đọc dữ liệu đã được đồng bộ sẵn trong Cache.

Nhờ vậy, Microservices gần như không cần thực hiện các lời gọi thời gian thực đến hệ thống nguồn mà vẫn đảm bảo dữ liệu luôn được cập nhật.

---

## AWS triển khai Use case 2 như thế nào?

![Blog2](/images/3-BlogsTranslated/Blog2_4.jpg)

> *Hình 4. Các dịch vụ AWS được sử dụng để triển khai Cache chủ động.*

Trong kiến trúc này, AWS thay đổi lớp Cache để phù hợp với khối lượng dữ liệu lớn.

Các thành phần chính bao gồm:

- **Amazon API Gateway** tiếp nhận request.
- **AWS Lambda** triển khai các Microservices.
- **Amazon DynamoDB** lưu trữ dữ liệu Cache lâu dài.
- **DynamoDB Accelerator (DAX)** tăng tốc các truy vấn đọc bằng cơ chế In-memory Cache.
- **Amazon EventBridge** truyền các sự kiện thay đổi dữ liệu.
- **Cache Manager** đồng bộ Cache gần như theo thời gian thực.

So với Use case đầu tiên, mô hình này phù hợp hơn với những hệ thống có lượng dữ liệu lớn hoặc yêu cầu dữ liệu luôn được cập nhật liên tục.

---

## Khi nào nên sử dụng từng chiến lược?

AWS không khuyến nghị một giải pháp duy nhất cho mọi hệ thống.

**Cache-Aside Pattern** phù hợp khi:

- Dữ liệu chủ yếu được đọc.
- Dữ liệu ít thay đổi.
- Muốn triển khai nhanh và đơn giản.

Trong khi đó, **Event-Driven Cache** phù hợp khi:

- Dữ liệu thay đổi thường xuyên.
- Yêu cầu tính nhất quán cao.
- Hệ thống sử dụng kiến trúc hướng sự kiện (Event-Driven Architecture).
- Khối lượng dữ liệu lớn và không thể truy vấn backend theo thời gian thực.

---

## Kết luận

Data Cache không chỉ giúp tăng tốc ứng dụng mà còn giảm tải đáng kể cho hệ thống backend trong kiến trúc Microservices.

Qua hai mô hình được AWS giới thiệu, có thể thấy mỗi chiến lược đều phục vụ một nhu cầu khác nhau. Cache-Aside đơn giản và dễ triển khai, trong khi Event-Driven Cache phù hợp với những hệ thống yêu cầu dữ liệu luôn mới và có lưu lượng truy cập lớn.

Điều quan trọng không phải là **có sử dụng Cache hay không, mà là lựa chọn chiến lược Cache phù hợp với đặc điểm của ứng dụng**. Đó cũng là thông điệp chính mà AWS muốn truyền tải trong bài viết này.
