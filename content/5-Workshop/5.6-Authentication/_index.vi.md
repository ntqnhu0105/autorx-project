---
title : "Thiết lập Authentication"
date : 2026-07-09
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ cấu hình **xác thực người dùng** cho hệ thống AutoRx bằng **Amazon Cognito**.

Amazon Cognito cung cấp dịch vụ quản lý người dùng và xác thực hoàn toàn được quản lý. Nó cho phép Web Dashboard đăng ký và xác thực người dùng, đồng thời cấp **JWT access token** để ủy quyền request đến API Gateway.

Luồng xác thực:

1. Người dùng đăng nhập qua Web Dashboard
2. Dashboard gửi thông tin đến **Amazon Cognito User Pool**
3. Cognito xác thực và trả về **JWT token**
4. Dashboard đính kèm JWT vào header `Authorization` của mỗi API request
5. **Amazon API Gateway** dùng **Cognito Authorizer** để xác thực token trước khi gọi Lambda

![Authentication Flow](/images/5-Workshop/5.6-Authentication/cognito-overview.png)

---

## 5.6.1 — Tạo Cognito User Pool

**User Pool** là thư mục người dùng trong Amazon Cognito. Nó lưu trữ tài khoản và xử lý đăng ký, đăng nhập, cấp token.

### Các bước

1. Mở [Cognito Console](https://console.aws.amazon.com/cognito)
2. Nhấn **Create user pool**
3. Ở **Sign-in experience**, chọn **Email** làm phương thức đăng nhập
4. Ở **Security requirements**, giữ nguyên password policy mặc định hoặc tùy chỉnh
5. Ở **Sign-up experience**, bật **Self-registration** nếu người dùng tự đăng ký
6. Ở **Message delivery**, cấu hình email (dùng Cognito default cho workshop)
7. Ở **Integrate your app**, điền:

| Trường | Giá trị |
|--------|---------|
| User pool name | `autorx-user-pool` |
| App client name | `autorx-web-client` |
| Authentication flows | `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH` |

8. Nhấn **Create user pool**

Sao chép **User Pool ID** và **Region** — bạn sẽ cần dùng sau.

![create-user-pool](/images/5-Workshop/5.6-Authentication/create-user-pool.png)

---

## 5.6.2 — Cấu hình App Client

**App Client** cho phép Web Dashboard tương tác với Cognito theo cách lập trình.

### Các bước

1. Trong Cognito console, mở `autorx-user-pool`
2. Vào **App integration** → **App clients**
3. Chọn `autorx-web-client`
4. Xác nhận các cài đặt:

| Cài đặt | Giá trị |
|---------|---------|
| Client secret | **Không có client secret** (public client) |
| Auth flows | `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH` |
| Token expiration | Access token: `1 giờ`, Refresh token: `30 ngày` |

5. Ở **Hosted UI**, đặt **Callback URL** thành domain CloudFront:
   ```
   https://<your-cloudfront-domain>/callback
   ```

Sao chép **App Client ID** — cấu hình Web Dashboard sẽ cần dùng.

![app-client](/images/5-Workshop/5.6-Authentication/app-client.png)

---

## 5.6.3 — Gắn Cognito Authorizer vào API Gateway

Cognito Authorizer đảm bảo chỉ người dùng đã xác thực mới gọi được các endpoint được bảo vệ.

### Các bước

1. Mở [API Gateway Console](https://console.aws.amazon.com/apigateway)
2. Chọn `autorx-rest-api`
3. Trong panel trái, nhấn **Authorizers** → **Create New Authorizer**
4. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Name | `autorx-cognito-authorizer` |
| Type | `Cognito` |
| Cognito User Pool | `autorx-user-pool` |
| Token Source | `Authorization` |

5. Nhấn **Create**
6. Nhấn **Test** và dán JWT token hợp lệ để xác nhận hoạt động đúng

### Gắn vào method

Với mỗi method cần bảo vệ (ví dụ `GET /vehicles`):

1. Chọn method → **Method Request**
2. Đặt **Authorization** thành `autorx-cognito-authorizer`
3. Nhấn dấu tick để lưu
4. Redeploy API: **Actions** → **Deploy API** → stage `prod`

![cognito-authorizer](/images/5-Workshop/5.6-Authentication/cognito-authorizer.png)

---

## 5.6.4 — Tạo Test User

Tạo người dùng test để xác minh luồng xác thực end-to-end.

### Các bước

1. Trong Cognito console, mở `autorx-user-pool`
2. Vào **Users** → **Create user**
3. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Email | `testuser@example.com` |
| Mật khẩu tạm | `Test@1234` |
| Đánh dấu email đã xác minh | ✓ |

4. Nhấn **Create user**

### Lấy JWT token qua CLI

```bash
aws cognito-idp initiate-auth \
  --auth-flow USER_PASSWORD_AUTH \
  --client-id <app-client-id> \
  --auth-parameters USERNAME=testuser@example.com,PASSWORD=Test@1234 \
  --region ap-southeast-1
```

Sao chép `IdToken` hoặc `AccessToken` từ response — dùng làm `Bearer` token trong các API call.

### Kiểm tra endpoint được bảo vệ

```bash
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/vehicles \
  -H "Authorization: Bearer <your-jwt-token>"
```

Kết quả mong đợi: HTTP `200` với dữ liệu phương tiện.

![test-user](/images/5-Workshop/5.6-Authentication/test-user.png)

---

### Kết quả

Amazon Cognito đã được cấu hình với User Pool, App Client và Authorizer gắn vào API Gateway. Hệ thống AutoRx hiện yêu cầu JWT token hợp lệ cho tất cả API call được bảo vệ.
