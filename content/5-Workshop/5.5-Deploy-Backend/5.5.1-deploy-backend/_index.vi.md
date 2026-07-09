---
title: "Triển khai Lambda API Handler"
date: 2026-07-09
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ triển khai **AutoRx API Handler** dưới dạng **AWS Lambda function**. Function này xử lý toàn bộ API request được chuyển tiếp từ Amazon API Gateway, truy vấn DynamoDB và trả về JSON response cho Web Dashboard.

![lambda-deploy](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/lambda-deploy.png)

---

### Cấu trúc Lambda Function

API Handler là một Lambda function duy nhất, định tuyến request theo path và HTTP method:

```
AutoRxAPIHandler/
├── index.js              → Entry point, request router
├── handlers/
│   ├── vehicles.js       → CRUD phương tiện
│   ├── telemetry.js      → Đọc dữ liệu telemetry
│   ├── maintenance.js    → Quản lý lịch bảo dưỡng
│   └── users.js          → Thông tin người dùng
└── lib/
    └── dynamodb.js       → DynamoDB client + tên bảng
```

---

### Bước 1 — Tạo Lambda Function

1. Mở **AWS Lambda Console**: https://console.aws.amazon.com/lambda
2. Nhấn **Create function**
3. Chọn **Author from scratch**
4. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Function name | `AutoRxAPIHandler` |
| Runtime | `Node.js 20.x` |
| Architecture | `x86_64` |

5. Ở phần **Permissions**, chọn **Create a new role with basic Lambda permissions**
6. Nhấn **Create function**

![create-lambda](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/create-lambda.png)

---

### Bước 2 — Upload Code

#### Cách A — Upload file ZIP

1. Trong Lambda console, vào tab **Code**
2. Nhấn **Upload from** → **.zip file**
3. Upload file `AutoRxAPIHandler.zip`
4. Nhấn **Save**

#### Cách B — Chỉnh sửa inline

Dùng trình soạn thảo tích hợp để dán code `index.js` trực tiếp.

Entry point phải export một hàm `handler`:

```javascript
export const handler = async (event) => {
  const { httpMethod, path, body, requestContext } = event;
  // định tuyến đến handler tương ứng
};
```

![upload-code](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/upload-code.png)

---

### Bước 3 — Gắn quyền IAM

Lambda execution role cần quyền truy cập DynamoDB.

1. Vào **Configuration** → **Permissions**
2. Nhấn vào **Role name** để mở IAM
3. Nhấn **Add permissions** → **Attach policies**
4. Tìm và gắn: **AmazonDynamoDBFullAccess**

> Trong môi trường production, nên tạo custom policy với quyền tối thiểu, giới hạn chỉ các bảng AutoRx.

![iam-policy](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/iam-policy.png)

---

### Bước 4 — Cấu hình cơ bản

1. Vào **Configuration** → **General configuration**
2. Thiết lập:

| Cài đặt | Giá trị |
|---------|---------|
| Memory | `256 MB` |
| Timeout | `15 seconds` |

3. Nhấn **Save**

---

### Bước 5 — Kiểm tra Function

1. Vào tab **Test**
2. Tạo test event với payload:

```json
{
  "httpMethod": "GET",
  "path": "/vehicles",
  "headers": {},
  "body": null
}
```

3. Nhấn **Test**
4. Xác nhận response trả về HTTP `200` với JSON body

![test-lambda](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/test-lambda.png)

---

### Kết quả

Lambda function `AutoRxAPIHandler` đã được triển khai và phản hồi test event thành công.

Tiếp theo, bạn sẽ cấu hình **Environment Variables** để function kết nối được với DynamoDB và các dịch vụ AWS khác.
