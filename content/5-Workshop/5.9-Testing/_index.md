---
title : "Test the System"
date : 2026-07-09
weight : 9
chapter : false
pre : " <b> 5.9. </b> "
---

#### Overview

In this section, you will perform an **end-to-end test** of the AutoRx system to verify all components are working together correctly.

The full data flow being tested:

1. **Vehicle Simulator** publishes telemetry via MQTT to **AWS IoT Core**
2. **IoT Rule** routes the message to **Amazon SQS**
3. **AWS Lambda (AutoRxTelemetryProcessor)** picks up the message, processes it, and writes to **DynamoDB**
4. If an anomaly is detected, **Amazon SNS** sends an email alert
5. The user logs in via the **Web Dashboard** using **Amazon Cognito**
6. The dashboard calls **Amazon API Gateway** with the JWT token
7. **AWS Lambda (AutoRxAPIHandler)** queries DynamoDB and returns data
8. The dashboard displays vehicle status, telemetry history, and maintenance records

![Testing Overview](/images/5-Workshop/5.9-Testing/testing-overview.png)

---

## 5.9.1 — Run the Vehicle Simulator

The Vehicle Simulator publishes MQTT telemetry messages to AWS IoT Core to simulate a real vehicle sending sensor data.

### Steps

1. Navigate to your simulator project directory
2. Ensure your IoT certificates are in the `certs/` folder
3. Run the simulator:

```bash
node simulator.js
```

The simulator will publish messages to the topic `vehicle/telemetry` every few seconds.

Expected console output:

```
[AutoRx Simulator] Connected to AWS IoT Core
[AutoRx Simulator] Published: { vehicleId: "default-vehicle-001", speed: 65, engineTemp: 88, ... }
[AutoRx Simulator] Published: { vehicleId: "default-vehicle-001", speed: 72, engineTemp: 91, ... }
```

![run-simulator](/images/5-Workshop/5.9-Testing/run-simulator.png)

---

## 5.9.2 — Verify Telemetry Data in DynamoDB

Confirm the Telemetry Processor Lambda is writing data to DynamoDB.

### Steps

1. Open the [DynamoDB Console](https://console.aws.amazon.com/dynamodb)
2. Go to **Tables** → `SmartVehicle_Telemetry`
3. Click **Explore table items**
4. Verify new records appear with the correct `vehicleId` and recent `timestamp`

Expected item structure:

```json
{
  "vehicleId": "default-vehicle-001",
  "timestamp": "2026-07-09T10:00:00.000Z",
  "speed": 65,
  "engineTemp": 88,
  "batteryVoltage": 12.4,
  "isAbnormal": false
}
```

![verify-dynamodb](/images/5-Workshop/5.9-Testing/verify-dynamodb.png)

---

## 5.9.3 — Check Lambda Logs in CloudWatch

Verify both Lambda functions are executing without errors.

### Steps

1. Open the [CloudWatch Console](https://console.aws.amazon.com/cloudwatch)
2. Go to **Logs → Log groups**
3. Open `/aws/lambda/AutoRxTelemetryProcessor`
4. Click the latest log stream
5. Confirm log entries show successful message processing:

```
START RequestId: ...
Processing 3 records from SQS
Wrote telemetry for vehicleId: default-vehicle-001
END RequestId: ...
REPORT Duration: 245.32 ms  Billed Duration: 246 ms  Memory Size: 256 MB
```

6. Open `/aws/lambda/AutoRxAPIHandler` and verify API call logs appear when the dashboard makes requests

![check-logs](/images/5-Workshop/5.9-Testing/check-logs.png)

---

## 5.9.4 — Test the Web Dashboard

Perform a full user flow test through the Web Dashboard.

### Steps

1. Open the CloudFront URL in your browser:
   ```
   https://xxxxxxxxxxxx.cloudfront.net
   ```
2. Sign in with the test user from Section 5.6.4
3. Verify the dashboard loads and shows:
   - Vehicle list with status indicators
   - Real-time telemetry data (updated as the simulator runs)
   - Maintenance schedule with upcoming service items
4. Navigate between pages to confirm all API calls succeed (no 401 or 500 errors in the browser console)

![test-dashboard](/images/5-Workshop/5.9-Testing/test-dashboard.png)

---

## 5.9.5 — Verify Email Notifications

Confirm that anomaly detection triggers SNS email alerts.

### Trigger an anomaly

Modify the simulator to send an out-of-range value, or manually insert an item with `isAbnormal: true` into DynamoDB:

```bash
aws dynamodb put-item \
  --table-name SmartVehicle_Telemetry \
  --item '{
    "vehicleId": {"S": "default-vehicle-001"},
    "timestamp": {"S": "2026-07-09T12:00:00.000Z"},
    "engineTemp": {"N": "120"},
    "isAbnormal": {"BOOL": true}
  }' \
  --region ap-southeast-1
```

### Verify

- Check your inbox for an alert email from `autorx-alerts`
- The email should include the vehicle ID and the anomaly details

![verify-notifications](/images/5-Workshop/5.9-Testing/verify-notifications.png)

---

### Result

The AutoRx system has passed end-to-end testing. Data flows correctly from the Vehicle Simulator through IoT Core, SQS, Lambda, and DynamoDB, and is displayed accurately on the Web Dashboard with alerting via SNS.
