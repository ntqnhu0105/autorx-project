---
title: "Blog 1"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# Xây dựng kiến trúc Multi-Region Active-Active với Amazon CloudFront

Các ứng dụng hiện đại ngày càng được triển khai trên nhiều AWS Region nhằm tăng tính sẵn sàng và giảm độ trễ cho người dùng trên toàn cầu. Tuy nhiên, việc triển khai backend trên nhiều Region chỉ là bước đầu. Một trong những thách thức lớn hơn là làm thế nào để người dùng luôn được kết nối đến Region phù hợp nhất mà không cần thay đổi địa chỉ truy cập.

Amazon CloudFront giúp đưa nội dung đến gần người dùng thông qua mạng lưới Edge Location toàn cầu. Tuy nhiên, khi Origin của CloudFront là **Amazon API Gateway Custom Domain**, việc lựa chọn Region dựa trên độ trễ (Latency-Based Routing) không được hỗ trợ trực tiếp.

Trong bài viết này, AWS giới thiệu một kiến trúc giúp kết hợp **Amazon CloudFront**, **Amazon Route 53**, **Amazon API Gateway** và **AWS Certificate Manager (ACM)** để xây dựng hệ thống **Multi-Region Active-Active**. Giải pháp này cho phép CloudFront vẫn sử dụng một endpoint duy nhất, trong khi Route 53 sẽ tự động định tuyến request đến Region có độ trễ thấp nhất.

---

## Kiến trúc tổng thể

Để hiện thực kiến trúc này, AWS bổ sung thêm một lớp định tuyến DNS phía sau CloudFront thay vì để CloudFront truy cập trực tiếp đến API Gateway của từng Region.

Thông qua Route 53, CloudFront chỉ cần giao tiếp với một tên miền duy nhất. Việc lựa chọn Region sẽ được Route 53 thực hiện bằng chính sách **Latency-Based Routing**, giúp người dùng luôn được chuyển đến Region có thời gian phản hồi tốt nhất.

![Blog1](/images/3-BlogsTranslated/Blog1_1.jpg)

> *Hình 1. Kiến trúc Multi-Region Active-Active sử dụng CloudFront, Route 53 và API Gateway Custom Domain.*

---

## Cấu hình DNS trong kiến trúc

Điểm quan trọng nhất của giải pháp nằm ở cách tổ chức DNS.

Thay vì cấu hình CloudFront trỏ trực tiếp đến API Gateway của từng Region, AWS sử dụng hai lớp tên miền:

- Domain chính (`example.com`) được cấu hình trong Route 53 và trỏ đến CloudFront Distribution.
- Origin Domain (`api.example.com`) được CloudFront sử dụng để truy cập backend.

Đối với `api.example.com`, Route 53 tạo nhiều bản ghi **Latency-Based Routing**, mỗi bản ghi tương ứng với một API Gateway Custom Domain của từng Region.

Ngoài ra, mỗi API Gateway Custom Domain đều được cấu hình chứng chỉ TLS riêng thông qua **AWS Certificate Manager (ACM)** để đảm bảo kết nối HTTPS giữa CloudFront và backend.

Cách tổ chức này giúp việc bổ sung hoặc loại bỏ Region trở nên đơn giản mà không ảnh hưởng đến người dùng cuối.

---

## Luồng xử lý request

Sau khi hoàn tất cấu hình DNS, request của người dùng sẽ được xử lý theo trình tự sau.

![Blog1](/images/3-BlogsTranslated/Blog1_2.jpg)

> *Hình 2. Luồng định tuyến request từ người dùng đến Region có độ trễ thấp nhất.*

Bước 1: Người dùng truy cập tên miền của ứng dụng (`example.com`).

Bước 2: Route 53 trả về địa chỉ của CloudFront Distribution.

Bước 3: Request được chuyển đến Edge Location gần người dùng nhất.

Bước 4: CloudFront tiếp tục truy vấn Route 53 thông qua Origin Domain (`api.example.com`).

Bước 5: Route 53 sử dụng **Latency-Based Routing** để lựa chọn API Gateway Custom Domain thuộc Region có độ trễ thấp nhất, sau đó request được chuyển đến backend để xử lý.

Toàn bộ quá trình diễn ra hoàn toàn tự động và minh bạch đối với người dùng.

---

## Những điểm cần lưu ý khi triển khai

Giải pháp của AWS mang lại nhiều lợi ích, tuy nhiên cũng có một số điểm cần cân nhắc:

- Mỗi Region cần triển khai API Gateway và Custom Domain riêng.
- Mỗi Custom Domain phải được cấp chứng chỉ TLS bằng AWS Certificate Manager.
- Route 53 đóng vai trò trung tâm trong việc định tuyến giữa các Region.
- CloudFront chỉ làm nhiệm vụ tiếp nhận request và chuyển tiếp đến Origin Domain, không trực tiếp quyết định Region nào sẽ xử lý request.
- Việc đồng bộ dữ liệu giữa các Region cần được thiết kế riêng tùy theo yêu cầu của ứng dụng.

---

## Lợi ích của kiến trúc

Việc bổ sung thêm lớp Route 53 phía sau CloudFront giúp giải quyết hạn chế của API Gateway Custom Domain khi triển khai Multi-Region.

Kiến trúc này mang lại nhiều lợi ích:

- Người dùng luôn được phục vụ từ Region có độ trễ thấp nhất.
- Hệ thống vẫn duy trì hoạt động khi một Region gặp sự cố.
- Chỉ cần một tên miền duy nhất cho toàn bộ ứng dụng.
- Dễ dàng mở rộng sang nhiều Region trong tương lai.
- Tận dụng được mạng lưới Edge Location của CloudFront để giảm thời gian phản hồi.
