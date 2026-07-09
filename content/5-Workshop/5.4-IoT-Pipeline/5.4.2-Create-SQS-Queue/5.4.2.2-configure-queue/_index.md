---
title : "Configure the Queue"
date : 2026-07-09
weight : 2
chapter : false
pre : " <b> 5.4.2.2 </b> "
---

After naming the queue, you will configure the Amazon SQS settings.

For this workshop, you can keep most of the default configuration values.

---

### Visibility Timeout

Set:

```text
30 Seconds
```

This timeout prevents multiple AWS Lambda functions from processing the same message simultaneously.

![visibility](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/configure-queue/visibility.jpg)

---

### Message Retention

Keep the default value:

```text
4 Days
```

This retention period allows messages to remain in the queue if processing is temporarily interrupted.

![retention](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/configure-queue/retention.jpg)

---

### Encryption

Select:

```text
Server-side encryption

↓

SSE-SQS
```

Amazon SQS will automatically encrypt messages stored in the queue.

![encryption](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/configure-queue/encryption.jpg)

---

### Complete the Configuration

Leave the remaining settings at their default values.

Choose **Create queue**.