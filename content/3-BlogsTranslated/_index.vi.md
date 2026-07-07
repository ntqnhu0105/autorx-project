---
title: "Các bài blogs đã dịch"
date: 2026-07-08
weight: 3
chapter: false
pre: " <b> 3. </b> "
---


### [Blog 1 - Latency-Based Routing: Leveraging Amazon CloudFront for a Multi-Region Active-Active Architecture](3.1-Blog1/)

Blog giới thiệu cách xây dựng kiến trúc **Multi-Region Active-Active** trên AWS nhằm đảm bảo ứng dụng luôn sẵn sàng và có độ trễ thấp cho người dùng trên toàn cầu. Bạn sẽ tìm hiểu vì sao triển khai trên nhiều Availability Zone vẫn chưa đủ để giải quyết các sự cố ở cấp Region, cũng như cách kết hợp **Amazon CloudFront**, **Amazon Route 53 (Latency-Based Routing)** và các dịch vụ backend để tự động định tuyến người dùng đến Region có độ trễ thấp nhất. Bài viết cũng phân tích cách hệ thống tiếp tục hoạt động khi một Region gặp sự cố, giúp doanh nghiệp nâng cao khả năng chịu lỗi, cải thiện trải nghiệm người dùng và xây dựng kiến trúc phân tán theo các AWS Best Practices.

### [Blog 2 - Data Caching Across Microservices in a Serverless Architecture](3.2-Blog2/)

Blog hướng dẫn cách triển khai **Data Cache** trong kiến trúc Microservices Serverless để giảm độ trễ và giảm tải cho hệ thống lưu trữ dữ liệu. Bạn sẽ tìm hiểu cách áp dụng **Cache-Aside Pattern**, cách AWS sử dụng **Amazon ElastiCache**, **Amazon EventBridge**, **AWS Lambda** và **Amazon DynamoDB** để xây dựng một hệ thống vừa có hiệu năng cao vừa đảm bảo tính nhất quán dữ liệu. Bài viết cũng phân tích các vấn đề thường gặp như **Cache Miss**, **Cache Consistency** và cách sử dụng kiến trúc hướng sự kiện (Event-Driven) để đồng bộ Cache gần như theo thời gian thực.

### [Blog 3 - Optimizing Your AWS Infrastructure for Sustainability – Part III: Networking](3.3-Blog3/)

Blog chia sẻ cách tối ưu **Network** trong hạ tầng AWS để cải thiện hiệu năng, giảm chi phí truyền dữ liệu và xây dựng hệ thống bền vững hơn. Bạn sẽ tìm hiểu vì sao tối ưu mạng không chỉ giúp giảm độ trễ mà còn góp phần giảm mức tiêu thụ tài nguyên, cách sử dụng **Amazon CloudFront** để đưa dữ liệu đến gần người dùng, cũng như cách theo dõi và tối ưu hệ thống bằng **Amazon CloudWatch** và **AWS Trusted Advisor**. Ngoài ra, bài viết còn giới thiệu các kỹ thuật như giảm kích thước dữ liệu truyền tải, lựa chọn Region phù hợp và tối ưu lưu lượng mạng theo các nguyên tắc của **AWS Well-Architected Framework – Sustainability Pillar**.