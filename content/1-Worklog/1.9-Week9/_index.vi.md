---
title: "Worklog Tuần 9"
date: 2026-06-21
weight: 1
chapter: false
pre: " <b> 1.9. </b> "
---


### Mục tiêu tuần 9:

* Tìm hiểu các phương pháp tối ưu hệ thống theo AWS Well-Architected Framework.
* Nâng cao độ tin cậy của hệ thống thông qua sao lưu và kết nối mạng.
* Triển khai và tự động hóa ứng dụng bằng các dịch vụ DevOps trên AWS.
* Tìm hiểu các giải pháp lưu trữ và tối ưu chi phí vận hành trên AWS.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu các phương pháp tối ưu hệ thống về **Độ tin cậy (Reliability)** <br> - **Thực hành:** <br>&emsp;+ Triển khai AWS Backup cho hệ thống <br>&emsp;+ Thiết lập VPC Peering <br>&emsp;+ Tìm hiểu AWS Transit Gateway và các trường hợp sử dụng | 15/06/2026 | 15/06/2026 | <https://000013.awsstudygroup.com/> <https://000013.awsstudygroup.com/3-createbackupplan/> <https://000013.awsstudygroup.com/4-enablenoti/> <https://000019.awsstudygroup.com/> <https://000019.awsstudygroup.com/3-updatenetworkacl/> <https://000019.awsstudygroup.com/4-vpcpeering/> <https://000019.awsstudygroup.com/5-routetables/> <https://000020.awsstudygroup.com/> |
| 3   | - Tìm hiểu các phương pháp tối ưu hệ thống về **Hiệu năng (Performance Efficiency)** <br> - **Thực hành:** <br>&emsp;+ Triển khai ứng dụng Docker trên AWS <br>&emsp;+ Xây dựng quy trình CI/CD với AWS CodePipeline <br>&emsp;+ Triển khai ứng dụng lên Amazon ECS | 16/06/2026 | 16/06/2026 | <https://000015.awsstudygroup.com/> <https://000015.awsstudygroup.com/2-deploy-local/> <https://000015.awsstudygroup.com/7-docker-compose/> <https://000016.awsstudygroup.com/> <https://000017.awsstudygroup.com/> |
| 4   | - **Thực hành:** <br>&emsp;+ Triển khai ứng dụng lên Amazon EC2 bằng AWS CodePipeline <br>&emsp;+ Tìm hiểu AWS Storage Gateway (File Storage Gateway) để mở rộng lưu trữ dữ liệu | 17/06/2026 | 17/06/2026 | <https://000023.awsstudygroup.com/> <https://000024.awsstudygroup.com/> <https://000024.awsstudygroup.com/2-useawsstoragegw/> |
| 5   | - Triển khai Amazon FSx for Windows File Server <br> - Tìm hiểu cách quản lý và chia sẻ dữ liệu trên Windows | 18/06/2026 | 18/06/2026 | <https://000025.awsstudygroup.com/> |
| 6   | - Tìm hiểu kiến trúc Data Lake trên AWS <br> - **Thực hành:** <br>&emsp;+ Xây dựng Data Lake cơ bản <br>&emsp;+ Quản lý và lưu trữ dữ liệu tập trung | 19/06/2026 | 19/06/2026 | <https://000035.awsstudygroup.com/> <https://000035.awsstudygroup.com/4-createdatacatalog/> <https://000035.awsstudygroup.com/4-createdatacatalog/> |
| 7   | - Tìm hiểu các giải pháp tối ưu chi phí trên AWS <br>&emsp;+ AWS Savings Plans <br>&emsp;+ Reserved Instances <br>&emsp;+ Reserved DB Instances <br>&emsp;+ Lựa chọn kích cỡ EC2 phù hợp (Right Sizing) <br>&emsp;+ Trực quan hóa và theo dõi chi phí bằng các công cụ AWS Cost Management | 20/06/2026 | 20/06/2026 | <https://000042.awsstudygroup.com/> <https://000042.awsstudygroup.com/2---savings-plan-recommendation/> <https://000042.awsstudygroup.com/4-reserved-instance/> <https://000042.awsstudygroup.com/5-reserved-db-instance/> <https://000032.awsstudygroup.com/> <https://000034.awsstudygroup.com/> |


### Kết quả đạt được tuần 9:

* Hiểu các nguyên tắc tối ưu hệ thống theo AWS Well-Architected Framework, đặc biệt ở các trụ cột:
  * Reliability.
  * Performance Efficiency.
  * Cost Optimization.

* Triển khai thành công AWS Backup để bảo vệ dữ liệu và nâng cao khả năng phục hồi hệ thống.

* Hiểu cách kết nối nhiều VPC thông qua:
  * VPC Peering.
  * AWS Transit Gateway.

* Triển khai ứng dụng sử dụng Docker trên AWS và làm quen với quy trình CI/CD thông qua AWS CodePipeline.

* Có khả năng triển khai ứng dụng lên:
  * Amazon ECS.
  * Amazon EC2 thông qua AWS CodePipeline.

* Hiểu vai trò của AWS Storage Gateway và Amazon FSx trong việc tích hợp và mở rộng hệ thống lưu trữ giữa môi trường on-premises và AWS.

* Nắm được các khái niệm cơ bản về Data Lake và xây dựng môi trường lưu trữ dữ liệu tập trung trên AWS.

* Hiểu các phương pháp tối ưu chi phí vận hành trên AWS:
  * AWS Savings Plans.
  * Reserved Instances.
  * Reserved DB Instances.
  * Right Sizing cho Amazon EC2.
  * Theo dõi và trực quan hóa chi phí bằng các công cụ quản lý chi phí của AWS.

* Nâng cao khả năng thiết kế hệ thống AWS đáp ứng yêu cầu về độ tin cậy, hiệu năng và tối ưu chi phí.
