---
title: "Kiểm tra CRUD với DynamoDB"
date: 2026-07-09
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ xác minh toàn bộ **luồng CRUD** bằng cách gửi request đến **API Gateway endpoint** và kiểm tra dữ liệu được ghi/đọc đúng trong **Amazon DynamoDB**.

Toàn bộ request đi qua: `Web Dashboard → API Gateway → Lambda → DynamoDB`

![crud-overview](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/crud-overview.png)

---

### Điều kiện tiên quyết

- API Gateway Invoke URL (từ Phần 5.5.3)
- JWT token hợp lệ từ Cognito (lấy sau khi đăng nhập qua Cognito — xem Phần 5.6)
- 4 bảng DynamoDB đã được tạo (xem Phần 5.3)

Khai báo biến shell để tiện sử dụng:

```bash
API="https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod"
TOKEN="<your-cognito-jwt-token>"
```

---

### CREATE — Thêm phương tiện mới

```bash
curl -X POST "$API/vehicles" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "licensePlate": "51G-888.88",
    "modelName": "Toyota Camry",
    "currentTotalKm": 5000
  }'
```

Kết quả mong đợi:

```json
{
  "success": true,
  "vehicle": {
    "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "licensePlate": "51G-888.88",
    "modelName": "Toyota Camry",
    "currentTotalKm": 5000,
    "status": "NORMAL"
  }
}
```

#### Kiểm tra trong DynamoDB Console

```
Amazon DynamoDB → Tables → SmartVehicle_Vehicles → Explore table items
```

Tìm item với `licensePlate = "51G-888.88"`.

![create-vehicle](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/create-vehicle.png)

---

### READ — Truy vấn danh sách phương tiện

```bash
curl "$API/vehicles" \
  -H "Authorization: Bearer $TOKEN"
```

Kết quả mong đợi:

```json
{
  "vehicles": [
    {
      "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "licensePlate": "51G-888.88",
      "modelName": "Toyota Camry",
      "currentTotalKm": 5000,
      "status": "NORMAL"
    }
  ]
}
```

Lambda dùng `ScanCommand` trên bảng `SmartVehicle_Vehicles` để trả về tất cả bản ghi.

#### Đọc Telemetry theo phương tiện

```bash
curl "$API/telemetry?vehicleId=<vehicle-id>&limit=20" \
  -H "Authorization: Bearer $TOKEN"
```

Lambda dùng `QueryCommand` với `KeyConditionExpression`:

```javascript
KeyConditionExpression: "vehicleId = :vid",
ExpressionAttributeValues: { ":vid": vehicleId },
ScanIndexForward: false,  // mới nhất trước
Limit: 20,
```

![read-telemetry](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/read-telemetry.png)

---

### UPDATE — Cập nhật thông tin phương tiện

```bash
curl -X PUT "$API/vehicles/<vehicle-id>" \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "modelName": "Toyota Camry 2026",
    "currentTotalKm": 5500,
    "status": "WARNING"
  }'
```

Kết quả mong đợi:

```json
{
  "success": true,
  "vehicle": {
    "id": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "licensePlate": "51G-888.88",
    "modelName": "Toyota Camry 2026",
    "currentTotalKm": 5500,
    "status": "WARNING"
  }
}
```

Lambda xây dựng `UpdateCommand` động:

```javascript
await dynamo.send(new UpdateCommand({
  TableName: TABLES.VEHICLES,
  Key: { id },
  UpdateExpression: "SET modelName = :m, currentTotalKm = :km, #s = :s",
  ExpressionAttributeNames: { "#s": "status" },
  ExpressionAttributeValues: {
    ":m": modelName,
    ":km": currentTotalKm,
    ":s": status,
  },
}));
```

#### Kiểm tra trong DynamoDB Console

```
SmartVehicle_Vehicles → Explore table items → tìm theo id → xác nhận các trường đã cập nhật
```

![update-vehicle](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/update-vehicle.png)

---

### DELETE — Xóa phương tiện

```bash
curl -X DELETE "$API/vehicles/<vehicle-id>" \
  -H "Authorization: Bearer $TOKEN"
```

Kết quả mong đợi:

```json
{ "success": true }
```

Lambda thực hiện **cascade delete**:
1. Query và batch-delete toàn bộ telemetry của phương tiện
2. Query và batch-delete toàn bộ lịch bảo dưỡng của phương tiện
3. Xóa item phương tiện khỏi `SmartVehicle_Vehicles`

#### Kiểm tra trong DynamoDB Console

Xác nhận không còn bản ghi telemetry:

```
SmartVehicle_Telemetry → Explore table items → filter: vehicleId = "<vehicle-id>"
```

Kết quả phải rỗng.

![delete-vehicle](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/delete-vehicle.png)

---

### Tóm tắt luồng dữ liệu

```
HTTP Request (kèm JWT)
        │
        ▼
Amazon API Gateway
  (xác thực JWT qua Cognito Authorizer)
        │
        ▼
AWS Lambda — AutoRxAPIHandler
  (định tuyến theo path + method, thực thi lệnh DynamoDB)
        │
        ▼
Amazon DynamoDB
  (PutItem / Query / UpdateItem / DeleteItem)
        │
        ▼
JSON Response → API Gateway → Web Dashboard
```

---

### Kết quả

Luồng CRUD đầy đủ giữa AutoRx API và Amazon DynamoDB hoạt động chính xác qua API Gateway.

Backend đã được triển khai hoàn chỉnh và sẵn sàng phục vụ Web Dashboard ở phần tiếp theo.
