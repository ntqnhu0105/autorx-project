---
title : "Create an Amazon SQS Queue"
date : 2026-07-09
weight : 2
chapter : false
pre : " <b> 5.4.2 </b> "
---

#### Overview

In this section, you will create an **Amazon SQS queue** to temporarily store telemetry messages before they are processed by **AWS Lambda**.

Using Amazon SQS decouples message ingestion from business logic processing. When telemetry traffic increases, messages are buffered in the queue instead of being sent directly to AWS Lambda. This improves the system's reliability, helps absorb traffic spikes, and reduces the risk of data loss.

#### Contents

- [Create an Amazon SQS Queue](5.4.2.1-create-queue/)
- [Configure the Queue](5.4.2.2-configure-queue/)
- [Verify the Queue](5.4.2.3-verify/)