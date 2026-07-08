---
title: "Blog 3"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# [Tối ưu hạ tầng AWS cho tính bền vững – Phần III: Networking](https://aws.amazon.com/blogs/architecture/optimizing-your-aws-infrastructure-for-sustainability-part-iii-networking/)

Khi nói đến tối ưu hạ tầng trên AWS, nhiều người thường tập trung vào việc lựa chọn loại EC2 phù hợp, tối ưu cơ sở dữ liệu hoặc giảm dung lượng lưu trữ.

Tuy nhiên, một yếu tố khác cũng ảnh hưởng trực tiếp đến hiệu năng, chi phí và mức tiêu thụ tài nguyên là **Network**.

Mỗi request của người dùng đều cần truyền dữ liệu qua mạng. Khoảng cách truyền càng xa, lượng dữ liệu càng lớn thì càng tiêu tốn nhiều tài nguyên mạng hơn. Điều này không chỉ làm tăng độ trễ mà còn khiến chi phí truyền dữ liệu (Data Transfer) tăng theo.

Trong phần thứ ba của chuỗi bài viết về **Optimizing your AWS Infrastructure for Sustainability**, AWS tập trung vào việc tối ưu tầng Networking, đồng thời giới thiệu các nguyên tắc và dịch vụ giúp giảm lượng dữ liệu truyền qua mạng, cải thiện hiệu năng và xây dựng hệ thống bền vững hơn.

---

## Vì sao cần tối ưu Networking?

Khác với Compute hay Storage, tầng Network thường ít được chú ý trong quá trình thiết kế hệ thống.

Tuy nhiên, mọi dữ liệu giữa người dùng và ứng dụng đều phải đi qua mạng. Khi số lượng người dùng tăng hoặc dữ liệu ngày càng lớn, lượng tài nguyên mạng tiêu thụ cũng tăng theo.

AWS cho rằng tối ưu Network không chỉ giúp:

- Giảm độ trễ (Latency)
- Giảm lượng Data Transfer
- Cải thiện trải nghiệm người dùng
- Giảm chi phí vận hành

mà còn góp phần sử dụng tài nguyên hiệu quả hơn, từ đó giảm tác động đến môi trường.

---

## AWS đề xuất tối ưu Networking theo hai hướng

Theo AWS, việc tối ưu tầng mạng nên tập trung vào hai mục tiêu chính:

### 1. Giảm quãng đường dữ liệu phải truyền
 Đưa dữ liệu đến gần người dùng nhất có thể để giảm thời gian truyền tải.

 Ví dụ:
 - Chọn AWS Region gần phần lớn người dùng.
 - Sử dụng nhiều Region nếu ứng dụng phục vụ người dùng toàn cầu.
 - Sử dụng CDN để phân phối nội dung.

### 2. Giảm kích thước dữ liệu cần truyền
 Bên cạnh việc rút ngắn khoảng cách truyền, AWS cũng khuyến nghị giảm lượng dữ liệu gửi qua mạng bằng cách:

 - Nén dữ liệu (Gzip, Brotli)
 - Chỉ trả về dữ liệu thực sự cần thiết
 - Cache nội dung tĩnh
 - Tối ưu payload của API

 Hai nguyên tắc này giúp giảm đáng kể tài nguyên mạng cần sử dụng cho mỗi request.

---

## Đưa dữ liệu đến gần người dùng hơn bằng CloudFront

![Blog3](/images/3-BlogsTranslated/Blog3_1.jpg)

> *Hình 1. So sánh truy cập trực tiếp đến Amazon S3 và truy cập thông qua Amazon CloudFront.*

Hình đầu tiên minh họa hai cách người dùng truy cập dữ liệu.

Ở phía bên trái, tất cả người dùng trên thế giới đều gửi request trực tiếp đến một **Amazon S3 Bucket** đặt tại một Region AWS.

Điều này đồng nghĩa với việc:

Request phải truyền quãng đường rất xa.
Origin Server xử lý toàn bộ lưu lượng truy cập.
Độ trễ tăng lên khi người dùng ở xa Region.
Chi phí Data Transfer cao hơn.

Ở phía bên phải, AWS sử dụng **Amazon CloudFront**.

CloudFront lưu bản sao dữ liệu tại các **Edge Location** và **Regional Edge Cache** trên toàn cầu.

Khi người dùng gửi request:

- CloudFront sẽ kiểm tra dữ liệu đã tồn tại trong Edge Cache hay chưa.
- Nếu có, dữ liệu được trả về ngay từ điểm gần người dùng nhất.
- Chỉ khi Cache không có dữ liệu, CloudFront mới truy cập đến Origin để lấy dữ liệu.

Nhờ vậy:

- Giảm khoảng cách truyền dữ liệu.
- Giảm độ trễ.
- Giảm số lượng request đến Origin.
- Tiết kiệm băng thông và chi phí truyền dữ liệu.
- Tăng khả năng mở rộng của hệ thống.

Đây cũng là lý do CloudFront không chỉ là một CDN mà còn là một công cụ giúp tối ưu hiệu năng và tính bền vững của hạ tầng AWS.

---

## Theo dõi hệ thống để biết nên tối ưu ở đâu

![Blog3](/images/3-BlogsTranslated/Blog3_2.jpg)

> *Hình 2. CloudWatch và Trusted Advisor hỗ trợ giám sát các thành phần trong hệ thống.*

Sau khi tối ưu Network, câu hỏi tiếp theo là:

**Làm thế nào biết hệ thống đã thực sự được tối ưu?**

AWS đề xuất kết hợp **Amazon CloudWatch** và **AWS Trusted Advisor** để theo dõi và đánh giá toàn bộ hạ tầng.

Trong kiến trúc này:

- Các dịch vụ Compute như **Amazon EC2**, **Amazon ECS** và **Amazon EMR** gửi các chỉ số hoạt động đến CloudWatch.
- Các dịch vụ Storage như **Amazon S3**, **Amazon EBS**, **Amazon EFS** và **Amazon FSx** cũng cung cấp các chỉ số về lưu lượng và mức sử dụng tài nguyên.
- **Amazon CloudFront** gửi các chỉ số như Cache Hit Ratio, lưu lượng truy cập và độ trễ.

CloudWatch đóng vai trò thu thập và hiển thị toàn bộ các chỉ số này.

Trong khi đó, **AWS Trusted Advisor** sẽ phân tích dữ liệu và đưa ra các khuyến nghị như:

- Có nên sử dụng CloudFront cho S3 hay không.
- Bucket nào đang truyền quá nhiều dữ liệu trực tiếp.
- Thành phần nào có thể tối ưu để giảm lưu lượng mạng.
- Những cơ hội giúp giảm chi phí và cải thiện hiệu năng.

Thay vì tối ưu theo cảm tính, doanh nghiệp có thể dựa trên số liệu thực tế để đưa ra quyết định chính xác hơn.                      |

---

## Những khuyến nghị khác từ AWS

Ngoài việc sử dụng CloudFront, AWS cũng đưa ra một số khuyến nghị giúp giảm lưu lượng truyền qua mạng:

- Chọn AWS Region gần phần lớn người dùng.
- Sử dụng nhiều Region nếu ứng dụng phục vụ khách hàng toàn cầu.
- Nén dữ liệu trước khi truyền bằng Gzip hoặc Brotli.
- Giảm kích thước payload của API.
- Cache phản hồi API khi phù hợp.
- Theo dõi thường xuyên các chỉ số mạng bằng CloudWatch.
- Sử dụng Trusted Advisor để phát hiện các cơ hội tối ưu.

Mặc dù mỗi thay đổi có thể chỉ cải thiện một phần nhỏ, nhưng khi kết hợp lại sẽ tạo ra khác biệt đáng kể về hiệu năng, chi phí và mức sử dụng tài nguyên.

---

## Kết luận

Networking không chỉ là lớp kết nối giữa người dùng và ứng dụng mà còn ảnh hưởng trực tiếp đến hiệu năng, chi phí và khả năng mở rộng của toàn bộ hệ thống.

Qua bài viết này, AWS nhấn mạnh rằng việc tối ưu Network không đơn thuần là giảm độ trễ, mà còn là sử dụng tài nguyên mạng hiệu quả hơn thông qua các kỹ thuật như đưa dữ liệu đến gần người dùng, giảm lượng dữ liệu truyền tải và liên tục theo dõi các chỉ số vận hành.

Đó cũng là một trong những nguyên tắc quan trọng để xây dựng kiến trúc AWS vừa **hiệu quả, tiết kiệm chi phí và bền vững** trong dài hạn.
