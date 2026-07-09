---
title : "Triển khai Web Dashboard"
date : 2026-07-09
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ triển khai **AutoRx Web Dashboard** bằng **Amazon S3** để host file tĩnh và **Amazon CloudFront** làm CDN.

Dashboard là ứng dụng single-page (SPA) kết nối đến API Gateway bằng JWT token từ Cognito để hiển thị trạng thái xe, lịch sử telemetry và lịch bảo dưỡng.

![Dashboard Architecture](/images/5-Workshop/5.7-Web-Dashboard/dashboard-overview.png)

---

## 5.7.1 — Tạo S3 Bucket cho Static Hosting

### Các bước

1. Mở [S3 Console](https://console.aws.amazon.com/s3)
2. Nhấn **Create bucket**
3. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Bucket name | `autorx-web-dashboard` |
| Region | `ap-southeast-1` |
| Block all public access | **Bỏ chọn** (sẽ dùng CloudFront OAC) |

4. Nhấn **Create bucket**
5. Mở bucket → vào **Properties** → **Static website hosting**
6. Nhấn **Edit** → bật **Static website hosting**
7. Đặt **Index document** là `index.html` và **Error document** là `index.html`
8. Nhấn **Save changes**

![create-s3](/images/5-Workshop/5.7-Web-Dashboard/create-s3.png)

---

## 5.7.2 — Upload các file Dashboard

### Build dashboard

Tại project local, tạo bản build production:

```bash
npm run build
```

Output sẽ nằm trong thư mục `out/` hoặc `dist/` tùy framework.

### Upload lên S3

**Cách A — AWS Console**

1. Mở bucket `autorx-web-dashboard`
2. Nhấn **Upload** → **Add files** / **Add folder**
3. Chọn toàn bộ file từ thư mục build output
4. Nhấn **Upload**

**Cách B — AWS CLI**

```bash
aws s3 sync ./out s3://autorx-web-dashboard --delete
```

![upload-files](/images/5-Workshop/5.7-Web-Dashboard/upload-files.png)

---

## 5.7.3 — Tạo CloudFront Distribution

### Các bước

1. Mở [CloudFront Console](https://console.aws.amazon.com/cloudfront)
2. Nhấn **Create distribution**
3. Ở phần **Origin**, điền:

| Trường | Giá trị |
|--------|---------|
| Origin domain | Chọn `autorx-web-dashboard.s3.ap-southeast-1.amazonaws.com` |
| Origin access | **Origin access control settings (recommended)** |
| Create new OAC | Nhấn **Create new OAC** → giữ mặc định → **Create** |

4. Ở **Default cache behavior**:
   - Viewer protocol policy: **Redirect HTTP to HTTPS**
   - Allowed HTTP methods: **GET, HEAD**

5. Ở **Settings**:
   - Default root object: `index.html`
   - Price class: **Use only North America and Europe** (hoặc All để giảm độ trễ)

6. Nhấn **Create distribution**

### Cập nhật Bucket Policy

Sau khi tạo distribution, CloudFront sẽ nhắc cập nhật S3 bucket policy. Nhấn **Copy policy** rồi:

1. Quay lại S3 → `autorx-web-dashboard` → **Permissions** → **Bucket policy**
2. Dán và lưu policy

![cloudfront](/images/5-Workshop/5.7-Web-Dashboard/cloudfront.png)

---

## 5.7.4 — Cấu hình môi trường Dashboard

Trước khi upload (hoặc sau khi build lại), cấu hình dashboard trỏ đến các dịch vụ AWS đã triển khai.

Trong file cấu hình môi trường của project (ví dụ `.env.production` hoặc `config.js`):

```env
NEXT_PUBLIC_API_URL=https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod
NEXT_PUBLIC_COGNITO_REGION=ap-southeast-1
NEXT_PUBLIC_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
NEXT_PUBLIC_COGNITO_CLIENT_ID=<app-client-id>
```

Build lại và upload lên S3 sau khi cập nhật các giá trị này.

![configure-env](/images/5-Workshop/5.7-Web-Dashboard/configure-env.png)

---

## 5.7.5 — Truy cập Web Dashboard

1. Trong CloudFront console, sao chép **Distribution domain name**:
   ```
   https://xxxxxxxxxxxx.cloudfront.net
   ```
2. Mở trong trình duyệt
3. Bạn sẽ thấy trang đăng nhập AutoRx
4. Đăng nhập bằng test user đã tạo ở Phần 5.6.4
5. Xác nhận dashboard tải và hiển thị dữ liệu phương tiện từ DynamoDB

![dashboard-live](/images/5-Workshop/5.7-Web-Dashboard/dashboard-live.png)

---

### Kết quả

AutoRx Web Dashboard đã hoạt động trên CloudFront. Người dùng truy cập qua HTTPS, xác thực với Cognito và lấy dữ liệu an toàn qua API Gateway và Lambda.
