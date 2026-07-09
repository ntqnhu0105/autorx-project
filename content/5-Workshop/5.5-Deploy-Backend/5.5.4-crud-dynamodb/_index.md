---
title: "Test CRUD Operations with DynamoDB"
date: 2026-07-09
weight: 4
chapter: false
pre: " <b> 5.5.4. </b> "
---

#### Overview

In this section, you will verify the full **CRUD flow** by sending requests to the **API Gateway endpoint** and confirming the data is correctly written to and read from **Amazon DynamoDB**.

All requests go through: `Web Dashboard → API Gateway → Lambda → DynamoDB`

![crud-overview](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/crud-overview.png)

---

### Prerequisites

- API Gateway Invoke URL (from Section 5.5.3)
- A valid Cognito JWT token (obtained after sign-in via Cognito — see Section 5.6)
- All 4 DynamoDB tables created (see Section 5.3)

Set these as shell variables for convenience:

```bash
API="https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod"
TOKEN="<your-cognito-jwt-token>"
```

---

### CREATE — Add a New Vehicle

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

Expected response:

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

#### Verify in DynamoDB Console

```
Amazon DynamoDB → Tables → SmartVehicle_Vehicles → Explore table items
```

Find the item by `licensePlate = "51G-888.88"`.

![create-vehicle](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/create-vehicle.png)

---

### READ — Query Vehicles

```bash
curl "$API/vehicles" \
  -H "Authorization: Bearer $TOKEN"
```

Expected response:

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

The Lambda uses a `ScanCommand` on `SmartVehicle_Vehicles` to return all records.

#### Read Telemetry by Vehicle

```bash
curl "$API/telemetry?vehicleId=<vehicle-id>&limit=20" \
  -H "Authorization: Bearer $TOKEN"
```

The Lambda uses a `QueryCommand` with `KeyConditionExpression`:

```javascript
KeyConditionExpression: "vehicleId = :vid",
ExpressionAttributeValues: { ":vid": vehicleId },
ScanIndexForward: false,  // newest first
Limit: 20,
```

![read-telemetry](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/read-telemetry.png)

---

### UPDATE — Update Vehicle Information

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

Expected response:

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

The Lambda builds a dynamic `UpdateCommand`:

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

#### Verify in DynamoDB Console

```
SmartVehicle_Vehicles → Explore table items → find by id → confirm updated fields
```

![update-vehicle](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/update-vehicle.png)

---

### DELETE — Delete a Vehicle

```bash
curl -X DELETE "$API/vehicles/<vehicle-id>" \
  -H "Authorization: Bearer $TOKEN"
```

Expected response:

```json
{ "success": true }
```

The Lambda performs a **cascade delete**:
1. Query and batch-delete all telemetry records for the vehicle
2. Query and batch-delete all maintenance records for the vehicle
3. Delete the vehicle item from `SmartVehicle_Vehicles`

#### Verify in DynamoDB Console

Check that no telemetry records remain:

```
SmartVehicle_Telemetry → Explore table items → filter: vehicleId = "<vehicle-id>"
```

The result should be empty.

![delete-vehicle](/images/5-Workshop/5.5-BackendAPI/5.5.4-crud-dynamodb/delete-vehicle.png)

---

### Data Flow Summary

```
HTTP Request (with JWT)
        │
        ▼
Amazon API Gateway
  (validates JWT via Cognito Authorizer)
        │
        ▼
AWS Lambda — AutoRxAPIHandler
  (routes by path + method, executes DynamoDB command)
        │
        ▼
Amazon DynamoDB
  (PutItem / Query / UpdateItem / DeleteItem)
        │
        ▼
JSON Response → API Gateway → Web Dashboard
```

---

### Result

The full CRUD flow between the AutoRx API and Amazon DynamoDB is working correctly via API Gateway.

The backend is now fully deployed and ready to serve the Web Dashboard in the next section.
