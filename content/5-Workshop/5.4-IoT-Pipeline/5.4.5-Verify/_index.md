---
title : "Verify the Telemetry Pipeline"
date : 2026-07-09
weight : 5
chapter : false
pre : " <b> 5.4.5 </b> "
---

#### Overview

In this section, you will verify the complete **Telemetry Pipeline** after configuring **AWS IoT Core**, **Amazon SQS**, and **AWS Lambda**.

The goal is to confirm that telemetry data is successfully transmitted from the Vehicle Simulator to **Amazon DynamoDB**.

![verify-pipeline](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/verify-pipeline.jpg)

---

### Send Telemetry Data

Open the **Vehicle Simulator**.

Enter the simulation parameters and choose:

```text
Send Telemetry
```

The Vehicle Simulator will publish telemetry data to **AWS IoT Core** using the MQTT protocol.

![send-telemetry](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/telemetry.jpg)

---

### Verify Amazon SQS

Navigate to:

```text
Amazon SQS

↓

AutoRxTelemetryQueue

↓

Send and receive messages
```

Choose:

```text
Poll for messages
```

If the queue receives a new message, the AWS IoT Rule has successfully forwarded the telemetry data.

![queue-message](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/queue-message.jpg)

---

### Verify AWS Lambda

Navigate to:

```text
AWS Lambda

↓

AutoRxTelemetryProcessor

↓

Monitor

↓

View CloudWatch Logs
```

If the Lambda function is invoked successfully, the CloudWatch logs will display the telemetry processing details.

![lambda-log](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/lambda-log.jpg)

---

### Verify Amazon DynamoDB

Open:

```text
Amazon DynamoDB
```

Select the following table:

```text
SmartVehicle_Telemetry
```

Then choose:

```text
Explore table items
```

If a new telemetry record appears, the data has been successfully processed and stored.

![telemetry-table](/images/5-Workshop/5.4-IoT-Pipeline/5.4.5-verify/telemetry-table.jpg)

---

### Result

After completing these steps, the Telemetry Pipeline is fully operational.

Telemetry data is processed through the following architecture:

```text
Vehicle Simulator
        │
HTTP POST
        ▼
Next.js API
        │
MQTT Publish
        ▼
AWS IoT Core
        │
AWS IoT Rule
        ▼
Amazon SQS
        │
AWS Lambda
        ▼
Amazon DynamoDB
```

In the next section, you will deploy the **Backend REST API**, allowing the Dashboard and other applications to access data stored in the AutoRx system.