---
title: "Cấu hình Environment Variables"
date: 2026-07-09
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

#### Tổng quan

Lambda function `AutoRxAPIHandler` sử dụng **Environment Variables** để kết nối với DynamoDB và tham chiếu đúng tên bảng. Các biến này được cấu hình trực tiếp trong Lambda console — không cần file `.env`.

![env-config](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-config.png)

---

### Bước 1 — Mở Environment Variables

1. Mở **Lambda Console** và chọn `AutoRxAPIHandler`
2. Vào **Configuration** → **Environment variables**
3. Nhấn **Edit**

![env-edit](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-edit.png)

---

### Bước 2 — Thêm biến môi trường

Nhấn **Add environment variable** và nhập từng cặp key-value:

| Key | Value | Mô tả |
|-----|-------|-------|
| `AWS_REGION` | `ap-southeast-1` | Region triển khai DynamoDB |
| `TABLE_VEHICLES` | `SmartVehicle_Vehicles` | Tên bảng phương tiện |
| `TABLE_TELEMETRY` | `SmartVehicle_Telemetry` | Tên bảng telemetry |
| `TABLE_MAINTENANCE` | `SmartVehicle_Maintenance` | Tên bảng bảo dưỡng |
| `TABLE_USERS` | `SmartVehicle_Users` | Tên bảng người dùng |

> `AWS_ACCESS_KEY_ID` và `AWS_SECRET_ACCESS_KEY` **không cần thiết** — Lambda tự động dùng **IAM execution role** để xác thực với các dịch vụ AWS.

Nhấn **Save** khi hoàn tất.

![env-save](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-save.png)

---

### Bước 3 — Sử dụng biến trong code

Trong `lib/dynamodb.js`, dùng environment variables để khởi tạo client và ánh xạ tên bảng:

```javascript
import { DynamoDBClient } from "@aws-sdk/client-dynamodb";
import { DynamoDBDocumentClient } from "@aws-sdk/lib-dynamodb";

const client = new DynamoDBClient({
  region: process.env.AWS_REGION || "ap-southeast-1",
});

export const dynamo = DynamoDBDocumentClient.from(client);

export const TABLES = {
  VEHICLES:    process.env.TABLE_VEHICLES    || "SmartVehicle_Vehicles",
  TELEMETRY:   process.env.TABLE_TELEMETRY   || "SmartVehicle_Telemetry",
  MAINTENANCE: process.env.TABLE_MAINTENANCE || "SmartVehicle_Maintenance",
  USERS:       process.env.TABLE_USERS       || "SmartVehicle_Users",
};
```

---

### Kiểm tra

Sau khi lưu, quay lại **Configuration** → **Environment variables** để xác nhận đủ 5 biến đã được thêm đúng.

![env-verify](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-verify.png)

---

### Kết quả

Lambda function đã được cấu hình để kết nối đúng với các bảng DynamoDB trong region `ap-southeast-1`.

Tiếp theo, bạn sẽ expose các endpoint này qua **Amazon API Gateway**.
