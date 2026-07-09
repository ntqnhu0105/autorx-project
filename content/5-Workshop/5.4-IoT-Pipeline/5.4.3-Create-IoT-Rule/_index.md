---
title : "Create an AWS IoT Rule"
date : 2026-07-09
weight : 3
chapter : false
pre : " <b> 5.4.3 </b> "
---

#### Overview

In this section, you will create an **AWS IoT Rule** to automatically forward telemetry data from **AWS IoT Core** to **Amazon SQS**.

![IoT Rule Architecture](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/iot-rule-architecture.jpg)

---

### Create an AWS IoT Rule

- Open **AWS IoT Core**.
- From the navigation pane, choose **Message routing** → **Rules**.
- Choose **Create rule**.

![rules](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/rules.jpg)

---

### Configure the Rule

Enter the following information:

| Property | Value |
|----------|-------|
| Rule name | AutoRxTelemetryRule |
| Description | Route telemetry messages to Amazon SQS |

Then choose **Next**.

![rule-info](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/rule-info.jpg)

---

### Configure the SQL Statement

Enter the following SQL statement:

```sql
SELECT *
FROM 'autorx/telemetry'
```

Then choose **Next**.

![sql](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/sql.jpg)

---

### Configure the Rule Action

Select the following action:

```text
Amazon SQS Queue
```

Select the queue:

```text
AutoRxTelemetryQueue
```

For **Role**, choose or create:

```text
AutoRxIoTRuleRole
```

Then choose **Next**.

![action](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/rule-actions.jpg)

---

### Complete the Configuration

Review the configuration and choose **Create**.

![review](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/review.jpg)

---

### Verify the AWS IoT Rule

Open the **MQTT Test Client** in AWS IoT Core and subscribe to the following topic:

```text
autorx/telemetry
```

Publish a telemetry message from the Vehicle Simulator.

![mqtt-message](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/mqtt-message.jpg)

Next, open **Amazon SQS**, select **AutoRxTelemetryQueue**, and choose **Poll for messages**.

If a telemetry message appears in the queue, the AWS IoT Rule has been configured successfully.

![queue-message](/images/5-Workshop/5.4-IoT-Pipeline/5.4.3-create-iot-rule/queue-message.jpg)