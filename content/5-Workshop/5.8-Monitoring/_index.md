---
title : "Set Up Monitoring and Notifications"
date : 2026-07-09
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

#### Overview

In this section, you will configure **monitoring and alerting** for the AutoRx system using **Amazon CloudWatch** and **Amazon SNS**.

CloudWatch collects logs and metrics from Lambda functions. SNS sends email alerts when alarms are triggered or when the Telemetry Processor detects vehicle anomalies (e.g. high engine temperature, low battery voltage).

![Monitoring Architecture](/images/5-Workshop/5.8-Monitoring/monitoring-overview.png)

---

## 5.8.1 — Create an SNS Topic and Subscription

### Create the topic

1. Open the [SNS Console](https://console.aws.amazon.com/sns)
2. Click **Topics** → **Create topic**
3. Fill in:

| Field | Value |
|-------|-------|
| Type | `Standard` |
| Name | `autorx-alerts` |

4. Click **Create topic**

### Add an email subscription

1. Open `autorx-alerts` → click **Create subscription**
2. Fill in:

| Field | Value |
|-------|-------|
| Protocol | `Email` |
| Endpoint | Your email address |

3. Click **Create subscription**
4. Check your inbox and click **Confirm subscription** in the confirmation email

![sns-topic](/images/5-Workshop/5.8-Monitoring/sns-topic.png)

---

## 5.8.2 — Review Lambda CloudWatch Logs

Lambda automatically sends execution logs to CloudWatch. Use these to monitor function behavior and debug issues.

### View logs

1. Open the [CloudWatch Console](https://console.aws.amazon.com/cloudwatch)
2. Go to **Logs → Log groups**
3. Find and open:
   - `/aws/lambda/AutoRxAPIHandler`
   - `/aws/lambda/AutoRxTelemetryProcessor`
4. Click on a **Log stream** to view recent execution logs

### What to look for

- `START` / `END` / `REPORT` lines confirm successful invocations
- Error lines (`ERROR`, `Exception`) indicate issues to investigate
- `REPORT` lines show billed duration and memory usage

![cloudwatch-logs](/images/5-Workshop/5.8-Monitoring/cloudwatch-logs.png)

---

## 5.8.3 — Create CloudWatch Alarms

Create alarms to automatically notify when Lambda errors exceed acceptable thresholds.

### Alarm: API Handler errors

1. In CloudWatch, go to **Alarms** → **Create alarm**
2. Click **Select metric** → **Lambda** → **By Function Name**
3. Select `AutoRxAPIHandler` → metric `Errors`
4. Configure:

| Setting | Value |
|---------|-------|
| Statistic | `Sum` |
| Period | `5 minutes` |
| Threshold type | `Static` |
| Condition | `Greater than` `0` |

5. Under **Notification**, select **In alarm** → choose `autorx-alerts` SNS topic
6. Name the alarm: `AutoRxAPIHandler-Errors`
7. Click **Create alarm**

Repeat the same steps for `AutoRxTelemetryProcessor` → alarm name: `AutoRxTelemetryProcessor-Errors`.

![cloudwatch-alarm](/images/5-Workshop/5.8-Monitoring/cloudwatch-alarm.png)

---

## 5.8.4 — Verify Notifications

### Trigger a test alarm

Force a Lambda error or use the CloudWatch **Actions** menu to set the alarm state manually:

```bash
aws cloudwatch set-alarm-state \
  --alarm-name AutoRxAPIHandler-Errors \
  --state-value ALARM \
  --state-reason "Manual test" \
  --region ap-southeast-1
```

Check your inbox for an alert email from SNS within 1–2 minutes.

### Reset the alarm

```bash
aws cloudwatch set-alarm-state \
  --alarm-name AutoRxAPIHandler-Errors \
  --state-value OK \
  --state-reason "Test complete" \
  --region ap-southeast-1
```

![verify-alarm](/images/5-Workshop/5.8-Monitoring/verify-alarm.png)

---

### Result

CloudWatch alarms are active and connected to the `autorx-alerts` SNS topic. Any Lambda errors in the AutoRx system will trigger an email notification automatically.
