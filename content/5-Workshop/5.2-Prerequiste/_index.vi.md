---
title : "Các bước chuẩn bị"
date : 2024-01-01 
weight : 2
chapter : false
pre : " <b> 5.2. </b> "
---

Trong phần này, chúng ta sẽ chuẩn bị môi trường trước khi triển khai hệ thống AutoRx trên AWS.

Trước khi bắt đầu, hãy đảm bảo bạn đã chuẩn bị:

- Một tài khoản AWS hoặc AWS Academy Learner Lab.
- Một IAM User có đủ quyền để triển khai các dịch vụ trong workshop.
- Mã nguồn của dự án AutoRx.
- Git và Visual Studio Code đã được cài đặt trên máy tính.

Workshop sử dụng **Asia Pacific (Singapore)** (`ap-southeast-1`) để giảm độ trễ và hỗ trợ đầy đủ các dịch vụ AWS được sử dụng trong bài thực hành.

### IAM Permissions

Gắn chính sách quyền IAM dưới đây cho tài khoản người dùng IAM của bạn để có quyền triển khai và xóa các tài nguyên được tạo trong workshop này.

![IAM](/images/5-Workshop/5.2-Prerequisite/iam-permission.jpg)

### Chọn Region

Ở góc trên bên phải của AWS Console, chọn Region:

```
Asia Pacific (Singapore)
ap-southeast-1
```

Region này sẽ được sử dụng xuyên suốt workshop.

![region](/images/5-Workshop/5.2-Prerequisite/region.jpg)