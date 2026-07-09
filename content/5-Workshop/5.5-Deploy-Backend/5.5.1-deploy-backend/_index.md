---
title: "Deploy the Lambda API Handler"
date: 2026-07-09
weight: 1
chapter: false
pre: " <b> 5.5.1. </b> "
---

#### Overview

In this section, you will deploy the **AutoRx API Handler** as an **AWS Lambda function**. This function processes all API requests forwarded by Amazon API Gateway, queries DynamoDB, and returns structured JSON responses to the Web Dashboard.

![lambda-deploy](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/lambda-deploy.png)

---

### Lambda Function Structure

The API Handler is a single Lambda function that routes requests by path and HTTP method:

```
AutoRxAPIHandler/
├── index.js              → Entry point, request router
├── handlers/
│   ├── vehicles.js       → Vehicle CRUD operations
│   ├── telemetry.js      → Read telemetry data
│   ├── maintenance.js    → Maintenance schedule management
│   └── users.js          → User profile operations
└── lib/
    └── dynamodb.js       → DynamoDB client + table constants
```

---

### Step 1 — Create the Lambda Function

1. Open the **AWS Lambda Console**: https://console.aws.amazon.com/lambda
2. Click **Create function**
3. Select **Author from scratch**
4. Fill in:

| Field | Value |
|-------|-------|
| Function name | `AutoRxAPIHandler` |
| Runtime | `Node.js 20.x` |
| Architecture | `x86_64` |

5. Under **Permissions**, choose **Create a new role with basic Lambda permissions**
6. Click **Create function**

![create-lambda](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/create-lambda.png)

---

### Step 2 — Upload the Function Code

#### Option A — Upload ZIP file

1. In the Lambda console, go to the **Code** tab
2. Click **Upload from** → **.zip file**
3. Upload your `AutoRxAPIHandler.zip`
4. Click **Save**

#### Option B — Edit inline (for small functions)

Use the built-in code editor to paste your `index.js` directly.

The entry point must export a `handler` function:

```javascript
export const handler = async (event) => {
  const { httpMethod, path, body, requestContext } = event;
  // route to appropriate handler
};
```

![upload-code](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/upload-code.png)

---

### Step 3 — Attach IAM Permissions

The Lambda execution role needs permission to access DynamoDB.

1. Go to **Configuration** → **Permissions**
2. Click the **Role name** link to open IAM
3. Click **Add permissions** → **Attach policies**
4. Search for and attach: **AmazonDynamoDBFullAccess**

> For production, create a custom policy with least-privilege access restricted to only the AutoRx tables.

![iam-policy](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/iam-policy.png)

---

### Step 4 — Configure Basic Settings

1. Go to **Configuration** → **General configuration**
2. Set:

| Setting | Value |
|---------|-------|
| Memory | `256 MB` |
| Timeout | `15 seconds` |

3. Click **Save**

---

### Step 5 — Test the Function

1. Go to the **Test** tab
2. Create a new test event with this payload:

```json
{
  "httpMethod": "GET",
  "path": "/vehicles",
  "headers": {},
  "body": null
}
```

3. Click **Test**
4. Verify the response returns HTTP `200` with a JSON body

![test-lambda](/images/5-Workshop/5.5-BackendAPI/5.5.1-deploy/test-lambda.png)

---

### Result

The `AutoRxAPIHandler` Lambda function is deployed and responding to test events.

Next, you will configure the **Environment Variables** the function needs to connect to DynamoDB and other AWS services.
