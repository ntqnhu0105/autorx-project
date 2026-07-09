---
title : "Create an Amazon SQS Queue"
date : 2026-07-09
weight : 1
chapter : false
pre : " <b> 5.4.2.1 </b> "
---

In this step, you will create an **Amazon SQS Standard Queue** to temporarily store telemetry messages forwarded from **AWS IoT Core**.

Amazon SQS acts as a message queue that decouples message ingestion from data processing, improving the reliability and scalability of the system.

---

### Open Amazon SQS

From the **AWS Management Console**, search for:

```text
Amazon SQS
```

Then choose **Create queue**.

![sqs-home](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/create-queue/sqs-home.jpg)

---

### Select the Queue Type

Choose:

```text
Standard
```

This queue type is well suited for telemetry workloads because it provides high throughput and cost-effective message processing.

![queue-type](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/create-queue/queue-type.jpg)

---

### Name the Queue

Enter the following queue name:

```text
AutoRxTelemetryQueue
```

Then choose **Next**.

![queue-name](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/create-queue/queue-name.jpg)