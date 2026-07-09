---
title: "Thiết lập REST API với API Gateway"
date: 2026-07-09
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ tạo **REST API** trên **Amazon API Gateway** và kết nối với Lambda function `AutoRxAPIHandler`. API Gateway đóng vai trò cổng vào bảo mật cho toàn bộ request từ Web Dashboard.

![api-gateway-overview](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/api-gateway-overview.png)

---

### Bước 1 — Tạo REST API

1. Mở **API Gateway Console**: https://console.aws.amazon.com/apigateway
2. Nhấn **Create API**
3. Chọn **REST API** → **Build**
4. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| API name | `autorx-rest-api` |
| Description | `AutoRx Backend API` |
| Endpoint type | `Regional` |

5. Nhấn **Create API**

![create-api](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/create-api.png)

---

### Bước 2 — Tạo Resources và Methods

Bạn sẽ tạo cấu trúc route sau:

```
/vehicles          GET, POST
/vehicles/{id}     GET, PUT, DELETE
/telemetry         GET, POST
/maintenance       GET, POST
/users/{id}        GET, PUT
```

#### Tạo resource `/vehicles`

1. Trong API console, nhấn **Actions** → **Create Resource**
2. Đặt **Resource Name** là `vehicles`, **Resource Path** là `/vehicles`
3. Tích chọn **Enable API Gateway CORS**
4. Nhấn **Create Resource**

#### Thêm method GET vào `/vehicles`

1. Chọn resource `/vehicles`
2. Nhấn **Actions** → **Create Method** → chọn `GET` → nhấn dấu tick
3. Integration type: **Lambda Function**
4. Bật **Lambda Proxy Integration**
5. Lambda function: `AutoRxAPIHandler`
6. Nhấn **Save** → xác nhận dialog cấp quyền

Lặp lại cho `POST /vehicles`, sau đó tạo resource con `/vehicles/{id}` và thêm các method `GET`, `PUT`, `DELETE`.

![create-resource](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/create-resource.png)

---

### Bước 3 — Gắn Cognito Authorizer

Tất cả endpoint (trừ endpoint công khai) phải yêu cầu **JWT token hợp lệ từ Cognito**.

1. Trong panel bên trái, nhấn **Authorizers**
2. Nhấn **Create New Authorizer**
3. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Name | `autorx-cognito-authorizer` |
| Type | `Cognito` |
| Cognito User Pool | Chọn user pool AutoRx của bạn |
| Token Source | `Authorization` |

4. Nhấn **Create**

#### Gắn authorizer vào method

1. Chọn một method (ví dụ: `GET /vehicles`)
2. Nhấn **Method Request**
3. Đặt **Authorization** thành `autorx-cognito-authorizer`
4. Lặp lại cho tất cả method cần bảo vệ

![cognito-authorizer](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/cognito-authorizer.png)

---

### Bước 4 — Bật CORS

Với mỗi resource, bật CORS để Web Dashboard (host trên CloudFront) gọi được API:

1. Chọn resource (ví dụ: `/vehicles`)
2. Nhấn **Actions** → **Enable CORS**
3. Đặt **Access-Control-Allow-Origin** thành domain CloudFront hoặc `*` để test
4. Nhấn **Enable CORS and replace existing CORS headers**

![enable-cors](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/enable-cors.png)

---

### Bước 5 — Deploy API

1. Nhấn **Actions** → **Deploy API**
2. Chọn **[New Stage]**
3. Stage name: `prod`
4. Nhấn **Deploy**

Sao chép **Invoke URL** hiển thị ở đầu trang stage:

```
https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod
```

![deploy-api](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/deploy-api.png)

---

### Bước 6 — Kiểm tra Endpoint

Dùng `curl` hoặc Postman để test endpoint (trước khi gắn authorizer):

```bash
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/vehicles
```

Kết quả mong đợi:

```json
{
  "vehicles": []
}
```

Với endpoint được bảo vệ, thêm JWT token từ Cognito:

```bash
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/vehicles \
  -H "Authorization: Bearer <your-jwt-token>"
```

![test-api](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/test-api.png)

---

### Kết quả

REST API đã hoạt động tại Invoke URL của API Gateway. Tất cả route đã được kết nối với Lambda `AutoRxAPIHandler` và bảo vệ bằng Cognito Authorizer.

Tiếp theo, bạn sẽ xác minh toàn bộ **luồng CRUD** giữa API và DynamoDB.
