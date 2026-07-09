---
title : "Deploy the AWS Lambda Telemetry Processor"
date : 2026-07-09
weight : 4
chapter : false
pre : " <b> 5.4.4 </b> "
---

#### Overview

In this section, you will deploy the **AWS Lambda Telemetry Processor** to automatically process telemetry messages received from **Amazon SQS**.

AWS Lambda reads messages from the queue, processes the telemetry data, updates the vehicle status, and stores the results in **Amazon DynamoDB** tables.

![Lambda Architecture](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/lambda-architecture.jpg)

---

### Create an IAM Role for AWS Lambda

Open the **IAM** console.

Choose **Roles** → **Create role**.

Select:

```text
AWS Service
```

For the use case, choose:

```text
Lambda
```

Enter the following role name:

```text
AutoRxTelemetryLambdaRole
```

Then create the role.

![create-role](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/create-role.jpg)

---

### Attach Permissions to the Lambda Role

After creating the role, attach the following managed policies:

```text
AWSLambdaBasicExecutionRole
```

```text
AmazonDynamoDBFullAccess
```

If your application sends email notifications using Amazon SNS, also attach:

```text
AmazonSNSFullAccess
```

![attach-policy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/attach-policy.jpg)

---

### Create the Lambda Function

Open **AWS Lambda**.

Choose **Create function**.

Enter the following configuration:

| Property | Value |
|----------|-------|
| Function name | AutoRxTelemetryProcessor |
| Runtime | Node.js 22.x |
| Architecture | x86_64 |
| Execution role | AutoRxTelemetryLambdaRole |

Then choose **Create function**.

![create-function](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/create-function.jpg)

---

### Configure the Amazon SQS Trigger

Open the Lambda function you just created.

Choose:

```text
Add trigger
```

Configure the trigger as follows:

| Property | Value |
|----------|-------|
| Source | Amazon SQS |
| Queue | AutoRxTelemetryQueue |
| Batch size | 10 |

Then choose **Add**.

![add-trigger](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/add-trigger.jpg)

---

### Configure Environment Variables

Navigate to:

```text
Configuration

↓

Environment variables
```

Add the following environment variables:

| Key | Value |
|------|-------|
| REGION | ap-southeast-1 |
| VEHICLE_TABLE | SmartVehicle_Vehicles |
| TELEMETRY_TABLE | SmartVehicle_Telemetry |
| MAINTENANCE_TABLE | SmartVehicle_Maintenance |

Then choose **Save**.

![environment](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/env.jpg)

---

### Deploy the Source Code

In the **Code** section of the Lambda function, choose:

```text
Upload from

↓

.zip file
```

Select the Telemetry Processor source code package and choose **Deploy**.

![deploy](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/deploy_lambda.jpg)

---

### Verify the AWS Lambda Function

Return to the **Vehicle Simulator** and publish a new telemetry message.

After a few seconds, navigate to:

```text
AWS Lambda

↓

Monitor

↓

View CloudWatch Logs
```

If the Lambda function processes the message successfully, the CloudWatch logs will display the telemetry processing details.

![cloudwatch](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/cloudwatch.jpg)

---

Next, open **Amazon DynamoDB** and verify the following tables:

- SmartVehicle_Telemetry
- SmartVehicle_Vehicles
- SmartVehicle_Maintenance

If the data has been updated, the AWS Lambda function has successfully processed the telemetry messages.

![dynamodb](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/dynamodb1.jpg)

![dynamodb](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/dynamodb2.jpg)

![dynamodb](/images/5-Workshop/5.4-IoT-Pipeline/5.4.4-create-lambda/dynamodb3.jpg)