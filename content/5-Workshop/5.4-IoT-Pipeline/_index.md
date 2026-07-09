---
title : "Set Up the IoT Telemetry Pipeline"
date : 2026-07-09
weight : 4
chapter : false
pre : " <b> 5.4. </b> "
---

#### Overview

In this section, you will build the **Telemetry Pipeline** for the AutoRx system using AWS Serverless services.

The pipeline receives telemetry data from the **Vehicle Simulator** over the MQTT protocol. **AWS IoT Core** and an **AWS IoT Rule** then route the incoming messages to **Amazon SQS**. Finally, **AWS Lambda** processes the telemetry data, updates the vehicle status, and stores the results in **Amazon DynamoDB**.

Using Amazon SQS to decouple the processing components makes the system more resilient, reduces the risk of data loss during periods of high telemetry traffic, and enables AWS Lambda to process messages in batches.

![Telemetry Pipeline](/images/5-Workshop/5.4-IoT-Pipeline/pipeline-overview.jpg)

#### Contents

- [Create an AWS IoT Device](5.4.1-Create-IoT-Device/)
- [Configure an AWS IoT Rule](5.4.2-Create-IoT-Rule/)
- [Create an Amazon SQS Queue](5.4.3-Create-SQS-Queue/)
- [Deploy the AWS Lambda Telemetry Processor](5.4.4-Deploy-Telemetry-Lambda/)
- [Verify the Telemetry Pipeline](5.4.5-Verify/)