---
title: "Configure Environment Variables"
date: 2026-07-09
weight: 2
chapter: false
pre: " <b> 5.5.2. </b> "
---

#### Overview

The `AutoRxAPIHandler` Lambda function uses **Environment Variables** to connect to DynamoDB and reference the correct table names. These are configured directly in the Lambda console — no `.env` files needed.

![env-config](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-config.png)

---

### Step 1 — Open Environment Variables

1. Open the **Lambda Console** and select `AutoRxAPIHandler`
2. Go to **Configuration** → **Environment variables**
3. Click **Edit**

![env-edit](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-edit.png)

---

### Step 2 — Add Variables

Click **Add environment variable** and enter each key-value pair:

| Key | Value | Description |
|-----|-------|-------------|
| `AWS_REGION` | `ap-southeast-1` | DynamoDB deployment region |
| `TABLE_VEHICLES` | `SmartVehicle_Vehicles` | Vehicles table name |
| `TABLE_TELEMETRY` | `SmartVehicle_Telemetry` | Telemetry table name |
| `TABLE_MAINTENANCE` | `SmartVehicle_Maintenance` | Maintenance table name |
| `TABLE_USERS` | `SmartVehicle_Users` | Users table name |

> `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` are **not needed** — Lambda uses its **IAM execution role** to authenticate with AWS services automatically.

Click **Save** when done.

![env-save](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-save.png)

---

### Step 3 — Reference Variables in Code

In `lib/dynamodb.js`, use the environment variables to initialize the client and map table names:

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

### Verify

After saving, go back to **Configuration** → **Environment variables** to confirm all 5 variables are listed correctly.

![env-verify](/images/5-Workshop/5.5-BackendAPI/5.5.2-env/env-verify.png)

---

### Result

The Lambda function is now configured to connect to the correct DynamoDB tables in the `ap-southeast-1` region.

Next, you will expose these Lambda endpoints through **Amazon API Gateway**.
