---
title : "Deploy the Web Dashboard"
date : 2026-07-09
weight : 7
chapter : false
pre : " <b> 5.7. </b> "
---

#### Overview

In this section, you will deploy the **AutoRx Web Dashboard** using **Amazon S3** for static hosting and **Amazon CloudFront** as the CDN.

The dashboard is a static single-page application (SPA) that connects to the API Gateway endpoint using the Cognito JWT token to display vehicle status, telemetry history, and maintenance records.

![Dashboard Architecture](/images/5-Workshop/5.7-Web-Dashboard/dashboard-overview.png)

---

## 5.7.1 — Create an S3 Bucket for Static Hosting

### Steps

1. Open the [S3 Console](https://console.aws.amazon.com/s3)
2. Click **Create bucket**
3. Fill in:

| Field | Value |
|-------|-------|
| Bucket name | `autorx-web-dashboard` |
| Region | `ap-southeast-1` |
| Block all public access | **Uncheck** (we will use CloudFront OAC instead) |

4. Click **Create bucket**
5. Open the bucket → go to **Properties** → **Static website hosting**
6. Click **Edit** → enable **Static website hosting**
7. Set **Index document** to `index.html` and **Error document** to `index.html`
8. Click **Save changes**

![create-s3](/images/5-Workshop/5.7-Web-Dashboard/create-s3.png)

---

## 5.7.2 — Upload the Dashboard Files

### Build the dashboard

In your local project, create a production build:

```bash
npm run build
```

The output will be in the `out/` or `dist/` directory depending on your framework.

### Upload to S3

**Option A — AWS Console**

1. Open the `autorx-web-dashboard` bucket
2. Click **Upload** → **Add files** / **Add folder**
3. Select all files from your build output directory
4. Click **Upload**

**Option B — AWS CLI**

```bash
aws s3 sync ./out s3://autorx-web-dashboard --delete
```

![upload-files](/images/5-Workshop/5.7-Web-Dashboard/upload-files.png)

---

## 5.7.3 — Create a CloudFront Distribution

### Steps

1. Open the [CloudFront Console](https://console.aws.amazon.com/cloudfront)
2. Click **Create distribution**
3. Under **Origin**, fill in:

| Field | Value |
|-------|-------|
| Origin domain | Select `autorx-web-dashboard.s3.ap-southeast-1.amazonaws.com` |
| Origin access | **Origin access control settings (recommended)** |
| Create new OAC | Click **Create new OAC** → use defaults → **Create** |

4. Under **Default cache behavior**:
   - Viewer protocol policy: **Redirect HTTP to HTTPS**
   - Allowed HTTP methods: **GET, HEAD**

5. Under **Settings**:
   - Default root object: `index.html`
   - Price class: **Use only North America and Europe** (or All for lower latency)

6. Click **Create distribution**

### Update S3 Bucket Policy

After creating the distribution, CloudFront will prompt you to update the S3 bucket policy. Click **Copy policy** then:

1. Go back to S3 → `autorx-web-dashboard` → **Permissions** → **Bucket policy**
2. Paste and save the policy

![cloudfront](/images/5-Workshop/5.7-Web-Dashboard/cloudfront.png)

---

## 5.7.4 — Configure the Dashboard Environment

Before uploading (or after rebuilding), configure the dashboard to point to your deployed AWS services.

In your project's environment config file (e.g. `.env.production` or `config.js`):

```env
NEXT_PUBLIC_API_URL=https://<api-id>.execute-api.ap-southeast-1.amazonaws.com/prod
NEXT_PUBLIC_COGNITO_REGION=ap-southeast-1
NEXT_PUBLIC_COGNITO_USER_POOL_ID=ap-southeast-1_XXXXXXXXX
NEXT_PUBLIC_COGNITO_CLIENT_ID=<app-client-id>
```

Rebuild and re-upload to S3 after updating these values.

![configure-env](/images/5-Workshop/5.7-Web-Dashboard/configure-env.png)

---

## 5.7.5 — Access the Web Dashboard

1. In the CloudFront console, copy the **Distribution domain name**:
   ```
   https://xxxxxxxxxxxx.cloudfront.net
   ```
2. Open it in your browser
3. You should see the AutoRx login page
4. Sign in using the test user created in Section 5.6.4
5. Confirm the dashboard loads and displays vehicle data from DynamoDB

![dashboard-live](/images/5-Workshop/5.7-Web-Dashboard/dashboard-live.png)

---

### Result

The AutoRx Web Dashboard is live on CloudFront. Users access it via HTTPS, authenticate with Cognito, and retrieve data securely through API Gateway and Lambda.
