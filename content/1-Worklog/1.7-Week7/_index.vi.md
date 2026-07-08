---
title: "Worklog Tuần 7"
date: 2026-06-07
weight: 7
chapter: false
pre: " <b> 1.7. </b> "
---


### Mục tiêu tuần 7:

* Tìm hiểu các dịch vụ hỗ trợ vận hành (Operations) trên AWS.
* Thực hiện di chuyển máy chủ lên AWS bằng AWS VM Import/Export.
* Tự động hóa việc tối ưu chi phí và quản lý tài nguyên AWS.
* Làm quen với các công cụ giám sát, quản lý máy chủ và triển khai hạ tầng dưới dạng mã (Infrastructure as Code).

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tìm hiểu dịch vụ **AWS VM Import/Export** <br> - **Thực hành:** <br>&emsp;+ Nhập máy ảo (Virtual Machine) lên AWS <br>&emsp;+ Xuất EC2 Instance từ AWS <br>&emsp;+ Tham khảo video hướng dẫn và kiểm tra kết quả | 01/06/2026 | 01/06/2026 | <https://000014.awsstudygroup.com/> <https://000014.awsstudygroup.com/2-import-vm-to-aws/> <https://000014.awsstudygroup.com/3-export-vm-from-aws/> <https://000014.awsstudygroup.com/4-video/> |
| 3   | - Tìm hiểu cách tối ưu chi phí Amazon EC2 bằng AWS Lambda <br> - **Thực hành:** <br>&emsp;+ Gắn Tag cho EC2 Instance <br>&emsp;+ Tạo IAM Role cho Lambda <br>&emsp;+ Xây dựng Lambda Function để tự động quản lý EC2 <br>&emsp;+ Kiểm tra kết quả <br><br> - Làm quen với Grafana <br>&emsp;+ Chuẩn bị môi trường <br>&emsp;+ Cài đặt Grafana <br>&emsp;+ Giám sát hệ thống bằng Grafana | 02/06/2026 | 02/06/2026 | <https://000022.awsstudygroup.com/> <https://000022.awsstudygroup.com/2-prerequiste/> <https://000022.awsstudygroup.com/3-createtagforinstance/> <https://000022.awsstudygroup.com/4-createroleforlambda/> <https://000022.awsstudygroup.com/5-createlambdafunction/> <https://000022.awsstudygroup.com/6-testingresults/> <https://000029.awsstudygroup.com/1-introduce/> <https://000029.awsstudygroup.com/2-prerequiste/> <https://000029.awsstudygroup.com/3-installgrafana/> <https://000029.awsstudygroup.com/4-monitoringwithgrafana/> |
| 4   | | 4 | - Quản lý tài nguyên bằng Tag và Resource Groups <br> - **Thực hành:** <br>&emsp;+ Gắn Tag cho tài nguyên AWS <br>&emsp;+ Tạo Resource Group <br>&emsp;+ Quản lý quyền truy cập EC2 theo Resource Tag bằng IAM <br>&emsp;+ Tạo IAM Policy <br>&emsp;+ Tạo IAM Role <br>&emsp;+ Kiểm tra chính sách phân quyền | 03/06/2026 | 03/06/2026 | <https://000027.awsstudygroup.com/> <https://000027.awsstudygroup.com/1-using-tags/> <https://000027.awsstudygroup.com/2-resource-group/> <https://000028.awsstudygroup.com/> <https://000028.awsstudygroup.com/3-createiampolicy/> <https://000028.awsstudygroup.com/4-createiamrole/> <https://000028.awsstudygroup.com/5-checkpolicy/>  |
| 5   | - Tìm hiểu AWS Systems Manager <br> - **Thực hành:** <br>&emsp;+ Patch Manager <br>&emsp;+ Run Command để quản lý và cập nhật nhiều máy chủ EC2 cùng lúc | 04/06/2026 | 04/06/2026 | <https://000031.awsstudygroup.com/> <https://000031.awsstudygroup.com/3-patchmanager/> <https://000031.awsstudygroup.com/4-runcommand/> |
| 6   | - Làm việc với AWS Systems Manager Session Manager <br> - **Thực hành:** <br>&emsp;+ Kết nối đến EC2 không cần SSH <br>&emsp;+ Quản lý Session Logs <br>&emsp;+ Thiết lập Port Forwarding | 05/06/2026 | 05/06/2026 | <https://000058.awsstudygroup.com/> <https://000058.awsstudygroup.com/3-accessibilitytoinstances/> <https://000058.awsstudygroup.com/4-s3log/> <https://000058.awsstudygroup.com/5-portfwd/> |
| 7   | - Tìm hiểu AWS CloudFormation <br> - **Thực hành:** <br>&emsp;+ Các thành phần cơ bản của CloudFormation <br>&emsp;+ Xây dựng Template <br>&emsp;+ Triển khai hạ tầng bằng CloudFormation <br>&emsp;+ Tìm hiểu các tính năng nâng cao của CloudFormation | 06/06/2026 | 06/06/2026 | <https://000037.awsstudygroup.com/> <https://000037.awsstudygroup.com/3-cloudformationbasic/> <https://000037.awsstudygroup.com/4-cloudformationadvance/>|

### Kết quả đạt được tuần 7:

* Hiểu cách sử dụng AWS VM Import/Export để di chuyển máy ảo giữa môi trường on-premises và AWS.

* Biết cách sử dụng AWS Lambda để tự động hóa các tác vụ quản trị và tối ưu chi phí Amazon EC2.

* Làm quen với Grafana và triển khai hệ thống giám sát cơ bản cho hạ tầng AWS.

* Hiểu cách tổ chức và quản lý tài nguyên AWS bằng:
  * Resource Tags.
  * Resource Groups.
  * Chính sách IAM dựa trên Tag.

* Có khả năng quản lý và cập nhật nhiều máy chủ EC2 đồng thời bằng AWS Systems Manager:
  * Patch Manager.
  * Run Command.

* Thành thạo việc truy cập máy chủ EC2 thông qua Session Manager mà không cần mở cổng SSH, đồng thời quản lý nhật ký phiên làm việc và sử dụng Port Forwarding.

* Hiểu nguyên lý **Infrastructure as Code (IaC)** và triển khai hạ tầng AWS bằng AWS CloudFormation thông qua các Template.

* Nâng cao kỹ năng vận hành, quản trị và tự động hóa hạ tầng AWS theo các phương pháp thực hành tốt (AWS Operations Best Practices).
