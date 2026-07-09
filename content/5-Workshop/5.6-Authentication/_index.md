---
title : "Configure Authentication"
date : 2026-07-09
weight : 6
chapter : false
pre : " <b> 5.6. </b> "
---

#### Overview

In this section, you will configure **user authentication** for the AutoRx system using **Amazon Cognito**.

Amazon Cognito provides a fully managed user directory and authentication service. It allows the Web Dashboard to register and authenticate users, and issues **JWT access tokens** used to authorize requests to API Gateway.

The authentication flow:

1. The user logs in through the Web Dashboard
2. The dashboard sends credentials to the **Amazon Cognito User Pool**
3. Cognito validates the credentials and returns a **JWT token**
4. The dashboard includes the JWT in the `Authorization` header of every API request
5. **Amazon API Gateway** uses a **Cognito Authorizer** to validate the token before invoking Lambda

![Authentication Flow](/images/5-Workshop/5.6-Authentication/cognito-overview.png)

---

## 5.6.1 — Create a Cognito User Pool

A **User Pool** is a user directory in Amazon Cognito. It stores user accounts and handles sign-up, sign-in, and token issuance.

### Steps

1. Open the [Cognito Console](https://console.aws.amazon.com/cognito)
2. Click **Create user pool**
3. Under **Sign-in experience**, select **Email** as the sign-in option
4. Under **Security requirements**, keep the default password policy or customize as needed
5. Under **Sign-up experience**, enable **Self-registration** if users can register themselves
6. Under **Message delivery**, configure email delivery (use Cognito default for the workshop)
7. Under **Integrate your app**, fill in:

| Field | Value |
|-------|-------|
| User pool name | `autorx-user-pool` |
| App client name | `autorx-web-client` |
| Authentication flows | `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH` |

8. Click **Create user pool**

Copy the **User Pool ID** and **Region** — you will need them later.

![create-user-pool](/images/5-Workshop/5.6-Authentication/create-user-pool.png)

---

## 5.6.2 — Configure the App Client

The **App Client** allows the Web Dashboard to interact with Cognito programmatically.

### Steps

1. In the Cognito console, open `autorx-user-pool`
2. Go to **App integration** → **App clients**
3. Select `autorx-web-client`
4. Confirm the following settings:

| Setting | Value |
|---------|-------|
| Client secret | **No client secret** (public client) |
| Auth flows | `ALLOW_USER_PASSWORD_AUTH`, `ALLOW_REFRESH_TOKEN_AUTH` |
| Token expiration | Access token: `1 hour`, Refresh token: `30 days` |

5. Under **Hosted UI**, set the **Callback URL** to your CloudFront domain:
   ```
   https://<your-cloudfront-domain>/callback
   ```

Copy the **App Client ID** — the Web Dashboard config will need it.

![app-client](/images/5-Workshop/5.6-Authentication/app-client.png)

---

## 5.6.3 — Attach a Cognito Authorizer to API Gateway

The Cognito Authorizer ensures only authenticated users can call protected API endpoints.

### Steps

1. Open the [API Gateway Console](https://console.aws.amazon.com/apigateway)
2. Select `autorx-rest-api`
3. In the left panel, click **Authorizers** → **Create New Authorizer**
4. Fill in:

| Field | Value |
|-------|-------|
| Name | `autorx-cognito-authorizer` |
| Type | `Cognito` |
| Cognito User Pool | `autorx-user-pool` |
| Token Source | `Authorization` |

5. Click **Create**
6. Click **Test** and paste a valid JWT token to confirm it validates correctly

### Attach to methods

For each protected API method (e.g. `GET /vehicles`):

1. Select the method → **Method Request**
2. Set **Authorization** to `autorx-cognito-authorizer`
3. Click the checkmark to save
4. Redeploy the API: **Actions** → **Deploy API** → stage `prod`

![cognito-authorizer](/images/5-Workshop/5.6-Authentication/cognito-authorizer.png)

---

## 5.6.4 — Create a Test User

Create a test user to verify the authentication flow end-to-end.

### Steps

1. In the Cognito console, open `autorx-user-pool`
2. Go to **Users** → **Create user**
3. Fill in:

| Field | Value |
|-------|-------|
| Email | `testuser@example.com` |
| Temporary password | `Test@1234` |
| Mark email as verified | ✓ |

4. Click **Create user**

### Obtain a JWT token via CLI

```bash
aws cognito-idp initiate-auth \
  --auth-flow USER_PASSWORD_AUTH \
  --client-id <app-client-id> \
  --auth-parameters USERNAME=testuser@example.com,PASSWORD=Test@1234 \
  --region ap-southeast-1
```

Copy the `IdToken` or `AccessToken` from the response — use it as the `Bearer` token in API calls.

### Test a protected endpoint

```bash
curl https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod/vehicles \
  -H "Authorization: Bearer <your-jwt-token>"
```

Expected: HTTP `200` with vehicle data.

![test-user](/images/5-Workshop/5.6-Authentication/test-user.png)

---

### Result

Amazon Cognito is configured with a User Pool, App Client, and Authorizer attached to API Gateway. The AutoRx system now requires a valid JWT token for all protected API calls.
