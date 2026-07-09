---
title: "Monitoring & Alert"
date: 2026-07-09
weight: 5
chapter: false
pre: " <b> 5.5 </b> "
---

# System Monitoring & Alerting

To ensure smooth system operations and timely detection of critical vehicle incidents, the project integrates a fully automated Monitoring & Alerting flow based on the AWS ecosystem.

#### 1. Amazon CloudWatch Logs - Historical Logging
All core services (IoT Core, API Gateway, Lambda) are configured to push execution logs to **CloudWatch Logs**.
* Every event, from a vehicle connecting and publishing an MQTT message to a Lambda function checking the temperature, is recorded.
* This allows administrators to easily trace and debug errors if a message fails to process or the web application cannot reach the database.

![Cloudwatch](/images/5-Workshop/5.5-Monitoring-alert/send-telemetry.jpg)


#### 2. CloudWatch Metrics & Alarms - Anomaly Detection
Beyond logging, the project actively measures performance and telemetry (Metrics).
* **Metrics:** The system establishes counters to track vehicle message frequency and current engine temperatures.
* **Alarms:** A CloudWatch Alarm is configured to continuously monitor these metrics. If the engine temperature exceeds 105°C (or if Lambda repeatedly throws errors), the Alarm instantly transitions from `OK` to `ALARM` state.

#### 3. Amazon SNS - Emergency Notification Distribution
When a CloudWatch Alarm is triggered, or when the Lambda algorithm detects that a vehicle is overdue for maintenance, the system needs a way to communicate with the owner:
* **SNS Topic:** A communication channel (Topic) named `VehicleAlerts` is established.
* **Automated Email/SMS:** User or administrator email addresses are Subscribed to this Topic. Upon an error, Amazon SNS automatically broadcasts an emergency email (e.g., "WARNING: Engine temperature exceeds safety limits") to the user.

![SNS](/images/5-Workshop/5.5-Monitoring-alert/sns.jpg)