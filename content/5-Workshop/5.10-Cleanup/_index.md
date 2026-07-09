---
title : "Clean Up Resources"
date : 2026-07-09
weight : 10
chapter : false
pre : " <b> 5.10. </b> "
---

#### Overview

Congratulations on completing the **AutoRx workshop**!

In this final section, you will delete all AWS resources created during this workshop to avoid incurring ongoing charges.

> **Important**: Delete resources in the order listed below to avoid dependency errors.

---

### 1. Disable and Delete the CloudFront Distribution

1. Open the [CloudFront Console](https://console.aws.amazon.com/cloudfront)
2. Select the distribution created for the AutoRx Web Dashboard
3. Click **Disable** and wait until the status changes to `Disabled`
4. Click **Delete** and confirm

---

### 2. Delete the S3 Bucket

1. Open the [S3 Console](https://console.aws.amazon.com/s3)
2. Select the bucket used for the Web Dashboard static files
3. Click **Empty** → confirm, then click **Delete** → confirm

---

### 3. Delete the API Gateway

1. Open the [API Gateway Console](https://console.aws.amazon.com/apigateway)
2. Select `autorx-rest-api`
3. Click **Actions** → **Delete API** and confirm

---

### 4. Delete the Lambda Functions

1. Open the [Lambda Console](https://console.aws.amazon.com/lambda)
2. Delete the following functions:
   - `AutoRxAPIHandler`
   - `AutoRxTelemetryProcessor`

---

### 5. Delete the Cognito User Pool

1. Open the [Cognito Console](https://console.aws.amazon.com/cognito)
2. Select the User Pool created for AutoRx
3. Click **Delete user pool** and confirm

---

### 6. Delete the SQS Queue

1. Open the [SQS Console](https://console.aws.amazon.com/sqs)
2. Select the queue created for the telemetry pipeline
3. Click **Delete** and confirm

---

### 7. Delete the AWS IoT Resources

1. Open the [AWS IoT Core Console](https://console.aws.amazon.com/iot)
2. Go to **Manage → Things** → delete the Vehicle Simulator thing
3. Go to **Security → Certificates** → delete the associated certificate
4. Go to **Security → Policies** → delete the IoT policy
5. Go to **Message Routing → Rules** → delete the IoT rule

---

### 8. Delete the DynamoDB Tables

1. Open the [DynamoDB Console](https://console.aws.amazon.com/dynamodb)
2. Delete the following tables:
   - `SmartVehicle_Vehicles`
   - `SmartVehicle_Telemetry`
   - `SmartVehicle_Maintenance`
   - `SmartVehicle_Users`

---

### 9. Delete CloudWatch Logs and Alarms

1. Open the [CloudWatch Console](https://console.aws.amazon.com/cloudwatch)
2. Go to **Logs → Log groups**
3. Delete log groups for `/aws/lambda/AutoRxAPIHandler` and `/aws/lambda/AutoRxTelemetryProcessor`
4. Go to **Alarms** and delete any alarms created during this workshop

---

### 10. Delete the SNS Topic

1. Open the [SNS Console](https://console.aws.amazon.com/sns)
2. Select the topic created for AutoRx notifications
3. Click **Delete** and confirm

---

All AWS resources have been removed. You have successfully completed the AutoRx workshop.
