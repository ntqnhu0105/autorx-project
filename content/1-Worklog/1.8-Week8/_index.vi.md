---
title: "Worklog Tuần 8"
date: 2026-06-14
weight: 8
chapter: false
pre: " <b> 1.8. </b> "
---


### Mục tiêu tuần 8:

* Nâng cao kiến thức về bảo mật trên nền tảng AWS.
* Tìm hiểu các phương pháp quản lý danh tính và phân quyền người dùng.
* Làm quen với các dịch vụ hỗ trợ giám sát và bảo vệ hệ thống trên AWS.
* Hiểu cách bảo vệ dữ liệu thông qua cơ chế mã hóa và tường lửa ứng dụng.

### Các công việc cần triển khai trong tuần này:
| Thứ | Công việc                                                                                                                                                                                   | Ngày bắt đầu | Ngày hoàn thành | Nguồn tài liệu                            |
| --- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------ | --------------- | ----------------------------------------- |
| 2   | - Tiếp tục tìm hiểu các phương pháp tối ưu hệ thống về mặt bảo mật <br> - Nghiên cứu các AWS Security Best Practices | 08/06/2026 | 08/06/2026 | <https://docs.aws.amazon.com/security/> |
| 3   | - Tìm hiểu AWS IAM Identity Center <br> - **Thực hành:** <br>&emsp;+ Thiết lập IAM Identity Center <br>&emsp;+ Quản lý người dùng và nhóm <br>&emsp;+ Cấu hình quyền truy cập | 09/06/2026 | 09/06/2026 | <https://000012.awsstudygroup.com/> <https://000012.awsstudygroup.com/2-aws-cli-access/> <https://000012.awsstudygroup.com/3-using-time-based-access-control/> <https://000012.awsstudygroup.com/4-using-customer-managed-policies/> <https://000012.awsstudygroup.com/%CC%805-iam-identity-center-identity-store-apis/>|
| 4   | - Tìm hiểu IAM Permission Boundary <br> - **Thực hành:** <br>&emsp;+ Tạo Permission Boundary <br>&emsp;+ Giới hạn quyền của IAM User <br>&emsp;+ Kiểm tra hiệu lực của Permission Boundary | 10/06/2026 | 10/06/2026 | <https://000030.awsstudygroup.com/> <https://000030.awsstudygroup.com/3-createpolicy/> <https://000030.awsstudygroup.com/4-createiamuser/> <https://000030.awsstudygroup.com/5-checkuser/> |
| 5   | - Đào sâu về IAM Role và IAM Condition <br> - **Thực hành:** <br>&emsp;+ Tạo IAM Role với Condition <br>&emsp;+ Kiểm tra và đánh giá quyền truy cập theo điều kiện | 11/06/2026 | 11/06/2026 | <https://000044.awsstudygroup.com/> <https://000044.awsstudygroup.com/2-create-iam-group/> <https://000044.awsstudygroup.com/3-create-iam-user/> <https://000044.awsstudygroup.com/4-configure-iam-role-with-condition/> |
| 6   | - Tìm hiểu AWS Security Hub <br> - **Thực hành:** <br>&emsp;+ Kích hoạt Security Hub <br>&emsp;+ Xem và phân tích các Security Findings | 12/06/2026 | 12/06/2026 | <https://000018.awsstudygroup.com/> <https://000018.awsstudygroup.com/2-enable-sec-hub/> <https://000018.awsstudygroup.com/3-security-score/> |
| 7 | - Tìm hiểu và thực hành AWS Web Application Firewall (AWS WAF) <br>&emsp;+ Tạo Web ACL <br>&emsp;+ Cấu hình Rule bảo vệ ứng dụng web <br> - Nghiên cứu AWS Key Management Service (AWS KMS) <br>&emsp;+ Tìm hiểu cơ chế mã hóa dữ liệu ở trạng thái lưu trữ (Encryption at Rest) <br>&emsp;+ Quản lý khóa mã hóa (Customer Managed Keys) | 13/06/2026 | 13/06/2026 | <https://000026.awsstudygroup.com/> <https://000026.awsstudygroup.com/3-useawswaf/> <https://000033.awsstudygroup.com/> <https://000033.awsstudygroup.com/3-create-kms/> <https://000033.awsstudygroup.com/4-create-s3/> <https://000033.awsstudygroup.com/5-create-cloudtrail/> <https://000033.awsstudygroup.com/6-demo/>  |


### Kết quả đạt được tuần 8:

* Hiểu các nguyên tắc bảo mật và các khuyến nghị (Best Practices) khi triển khai hệ thống trên AWS.

* Triển khai thành công AWS IAM Identity Center và hiểu cơ chế quản lý truy cập tập trung:
  * User.
  * Group.
  * Permission Set.
  * Single Sign-On (SSO).

* Hiểu cách sử dụng IAM Permission Boundary để giới hạn quyền tối đa mà IAM User hoặc IAM Role có thể nhận được.

* Nắm được cách áp dụng IAM Role kết hợp với IAM Condition nhằm xây dựng các chính sách phân quyền linh hoạt và an toàn hơn.

* Biết cách sử dụng AWS Security Hub để:
  * Tổng hợp các cảnh báo bảo mật.
  * Theo dõi tình trạng tuân thủ.
  * Phân tích các Security Findings.

* Triển khai và cấu hình AWS WAF nhằm bảo vệ ứng dụng web trước các cuộc tấn công phổ biến như SQL Injection và Cross-site Scripting (XSS).

* Hiểu nguyên lý mã hóa dữ liệu trên AWS thông qua AWS KMS:
  * Encryption at Rest.
  * Customer Managed Keys (CMK).
  * Quản lý và sử dụng khóa mã hóa.

* Nâng cao kỹ năng thiết kế hệ thống AWS theo hướng bảo mật, đáp ứng các yêu cầu về quản lý danh tính, phân quyền và bảo vệ dữ liệu.


