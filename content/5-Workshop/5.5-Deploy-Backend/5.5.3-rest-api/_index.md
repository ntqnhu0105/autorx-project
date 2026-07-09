---
title: "Set Up REST API with API Gateway"
date: 2026-07-09
weight: 3
chapter: false
pre: " <b> 5.5.3. </b> "
---

#### Overview

In this section, you will create a **REST API** in **Amazon API Gateway** and connect it to the `AutoRxAPIHandler` Lambda function. API Gateway acts as the secure entry point for all requests from the Web Dashboard.

![api-gateway-overview](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/api-gateway-overview.png)

---

### Step 1 — Create the REST API

1. Open the **API Gateway Console**: https://console.aws.amazon.com/apigateway
2. Click **Create API**
3. Choose **REST API** → **Build**
4. Fill in:

| Field | Value |
|-------|-------|
| API name | `autorx-rest-api` |
| Description | `AutoRx Backend API` |
| Endpoint type | `Regional` |

5. Click **Create API**

![create-api](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/create-api.png)

---

### Step 2 — Create Resources and Methods

You will create the following route structure:

```
/vehicles          GET, POST
/vehicles/{id}     GET, PUT, DELETE
/telemetry         GET, POST
/maintenance       GET, POST
/users/{id}        GET, PUT
```

#### Create the `/vehicles` resource

1. In the API console, click **Actions** → **Create Resource**
2. Set **Resource Name** to `vehicles`, **Resource Path** to `/vehicles`
3. Check **Enable API Gateway CORS**
4. Click **Create Resource**

#### Add GET method to `/vehicles`

1. Select the `/vehicles` resource
2. Click **Actions** → **Create Method** → select `GET` → click the checkmark
3. Integration type: **Lambda Function**
4. Enable **Lambda Proxy Integration**
5. Lambda function: `AutoRxAPIHandler`
6. Click **Save** → confirm the permission dialog

Repeat these steps for `POST /vehicles`, then create `/vehicles/{id}` as a child resource and add `GET`, `PUT`, `DELETE` methods.

![create-resource](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/create-resource.png)

---

### Step 3 — Attach a Cognito Authorizer

All endpoints (except public ones) must require a valid **Cognito JWT token**.

1. In the left panel, click **Authorizers**
2. Click **Create New Authorizer**
3. Fill in:

| Field | Value |
|-------|-------|
| Name | `autorx-cognito-authorizer` |
| Type | `Cognito` |
| Cognito User Pool | Select your AutoRx user pool |
| Token Source | `Authorization` |

4. Click **Create**

#### Attach the authorizer to methods

1. Select a method (e.g., `GET /vehicles`)
2. Click **Method Request**
3. Set **Authorization** to `autorx-cognito-authorizer`
4. Repeat for all protected methods

![cognito-authorizer](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/cognito-authorizer.png)

---

### Step 4 — Enable CORS

For each resource, enable CORS so the Web Dashboard (hosted on CloudFront) can call the API:

1. Select a resource (e.g., `/vehicles`)
2. Click **Actions** → **Enable CORS**
3. Set **Access-Control-Allow-Origin** to your CloudFront domain or `*` for testing
4. Click **Enable CORS and replace existing CORS headers**

![enable-cors](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/enable-cors.png)

---

### Step 5 — Deploy the API

1. Click **Actions** → **Deploy API**
2. Select **[New Stage]**
3. Stage name: `prod`
4. Click **Deploy**

Copy the **Invoke URL** shown at the top of the stage:

```
https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod
```

![deploy-api](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/deploy-api.png)

---

### Step 6 — Test an Endpoint

Use `curl` or Postman to test a public endpoint (before attaching the authorizer):

```bash
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/vehicles
```

Expected response:

```json
{
  "vehicles": []
}
```

For protected endpoints, include the Cognito JWT token:

```bash
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/vehicles \
  -H "Authorization: Bearer <your-jwt-token>"
```

![test-api](/images/5-Workshop/5.5-BackendAPI/5.5.3-rest-api/test-api.png)

---

### Result

The REST API is live at the API Gateway Invoke URL. All routes are connected to the `AutoRxAPIHandler` Lambda and protected by the Cognito Authorizer.

Next, you will verify the full **CRUD flow** between the API and DynamoDB.
