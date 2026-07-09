---
title : "Deploy the Backend API"
date : 2026-07-09
weight : 5
chapter : false
pre : " <b> 5.5. </b> "
---

#### Overview

In this section, you will deploy the **Backend API** of the AutoRx system using **AWS Lambda** and **Amazon API Gateway**.

The backend handles all API requests from the **Web Dashboard**, including vehicle data queries, telemetry history, maintenance records, and user management — all backed by **Amazon DynamoDB**.

#### Architecture

The request flow works as follows:

1. The user accesses the Web Dashboard hosted on **Amazon S3 + CloudFront**
2. The dashboard authenticates via **Amazon Cognito** and receives a **JWT token**
3. API calls are made to **Amazon API Gateway** with the JWT in the `Authorization` header
4. API Gateway validates the token via a **Cognito Authorizer** and invokes the corresponding **AWS Lambda (API Handler)**
5. The Lambda function reads/writes data in **Amazon DynamoDB** and returns a JSON response

![Backend Architecture](/images/5-Workshop/5.5-BackendAPI/backend-overview.png)

#### Contents

- [5.5.1 - Deploy the Lambda API Handler](5.5.1-deploy-backend/)
- [5.5.2 - Configure Environment Variables](5.5.2-env/)
- [5.5.3 - Set Up REST API with API Gateway](5.5.3-rest-api/)
- [5.5.4 - Test CRUD Operations with DynamoDB](5.5.4-crud-dynamodb/)
