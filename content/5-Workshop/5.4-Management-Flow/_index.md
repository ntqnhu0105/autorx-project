---
title: "Management Flow (API Gateway & DynamoDB)"
date: 2026-07-09
weight: 4
chapter: false
pre: " <b> 5.4 </b> "
---

# Data Management Flow with API Gateway and DynamoDB

#### 1. Create Amazon DynamoDB Table
* Access Amazon DynamoDB and create a new table (e.g., `VehicleData`).
* Configure the **Partition Key** (e.g., `vehicleId` as String) to easily query data for specific vehicles.
* Set up additional attributes if needed (e.g., `timestamp` as a Sort Key to retrieve historical data over time).
* Choose On-demand capacity mode to save costs during the workshop.

![table](/images/5-Workshop/5.4-Management-flow/table.jpg)

#### 2. Create REST API with API Gateway
* Navigate to Amazon API Gateway and create a new **REST API** (or HTTP API depending on the architecture).
* Set up necessary **Resources** and **Methods**. For example:
  - Create the `/vehicle/{id}` resource.
  - Add a `GET` method to query detailed vehicle information.
* Enable **CORS** (Cross-Origin Resource Sharing) so the Frontend can call the API without browser block errors.

![API](/images/5-Workshop/5.4-Management-flow/API.jpg)

#### 3. Connect API to Lambda (Integration)
* Create a new **AWS Lambda function** that will handle the API logic.
* In the API Gateway Method configuration, select **Lambda Function** as the Integration type and point it to the newly created function.
* Grant this Lambda the appropriate IAM Policy to allow read (`dynamodb:GetItem`, `dynamodb:Query`) and write (`dynamodb:PutItem`) operations to the DynamoDB table.
* Deploy the API to a Stage (e.g., `dev` or `prod`) and save the **Invoke URL** to configure the Frontend.

![API](/images/5-Workshop/5.4-Management-flow/Lambda-api.jpg)