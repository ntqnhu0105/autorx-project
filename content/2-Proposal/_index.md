---
title: "Proposal"
date: 2026-07-08
weight: 2
chapter: false
pre: " <b> 2. </b> "
---


# AutoRx: Smart Vehicle Monitoring & Maintenance Reminder System
## AWS Serverless & IoT-enabled Real-time Telemetry and Intelligent Diagnostics Platform

### 1. Executive Summary
AutoRx is an advanced IoT-based vehicle monitoring and automated maintenance scheduling platform designed to address the challenges of vehicle health tracking and predictive maintenance. The system simulates real-time vehicle telemetry data (including speed, engine temperature, battery voltage, and odometer mileage) sent securely via MQTT. Utilizing AWS Serverless and IoT Core architectures, the system ingests, filters, and logs sensor telemetry data into a NoSQL database (DynamoDB). It features a responsive Next.js Web Dashboard for vehicle tracking and an automated alert system utilizing CloudWatch Alarms and Amazon SNS. Additionally, it integrates Google Gemini AI to provide real-time, context-aware troubleshooting guidelines for drivers during critical vehicle failures.

### 2. Problem Statement
*Current Challenges:*
Vehicle maintenance is often reactive rather than proactive. Drivers fail to notice subtle shifts in engine temperature or battery degradation until a complete breakdown occurs. Traditional logging methods and manual mileage tracking for routine services (e.g., oil changes, brake pads) are highly error-prone, leading to skipped maintenance and costly repairs. Moreover, commercial vehicle diagnostic scanners (OBD-II readers) are complex and do not offer automated cloud integration or user-friendly AI consultations.

*Proposed Solution:*
AutoRx offers a comprehensive cloud-native solution:
* **IoT Ingestion:** AWS IoT Core ingests real-time MQTT telemetry packets from simulated vehicle OBD-II sensors.
* **Resiliency & Scaling:** Amazon SQS buffers incoming message bursts, ensuring high data reliability.
* **Serverless Processing:** AWS Lambda acts as the compute engine to check error thresholds and write logs to Amazon DynamoDB.
* **Static Hosting & CDN:** S3 and CloudFront CDN host the Next.js Frontend Dashboard with low-latency delivery.
* **Identity Management:** Amazon Cognito secures user logins and issues JWT Tokens.
* **Intelligent Diagnostics:** Google Gemini API analyzes sensor anomalies and generates instant, actionable recovery advice for drivers.
* **Emergency Alerting:** Amazon CloudWatch monitors metrics and triggers email notifications via Amazon SNS.

*Benefits & Return on Investment (ROI):*
AutoRx reduces vehicle downtime by warning drivers about critical status changes (e.g., temperatures above 105°C) before severe engine damage occurs. The automated odometer tracker ensures scheduled maintenance is done exactly when needed, extending vehicle life. The system operates on a lightweight, serverless framework, costing an estimated $0.98/month under standard development usage on AWS, maximizing resource efficiency.

### 3. Solution Architecture
The platform implements an end-to-end AWS Serverless architecture designed to process, store, and visualize vehicle telemetry.

![System Architecture](/images/2-Proposal/architect.jpg)

*AWS Services Utilized:*
* **AWS IoT Core:** Manages secure device registration, certificates, and MQTT message ingestion.
* **AWS IoT Rule Engine:** Filters raw MQTT topics and routes payloads to SQS.
* **Amazon SQS (Standard Queue):** Decouples ingestion and processing, buffering data to prevent loss.
* **AWS Lambda:** Hosts serverless backend functions (Telemetry Processor and API Handler).
* **Amazon DynamoDB (Shared Data Layer):** Stores vehicle profiles, real-time sensor logs, and maintenance statuses.
* **Amazon S3 & CloudFront:** Serves static frontend dashboard pages globally with CDN caching.
* **Amazon Cognito:** Controls user registration and authenticates API access via JWT.
* **Amazon API Gateway:** Exposes secure REST endpoints to the Web Dashboard.
* **Amazon CloudWatch:** Performs centralized logging, metric collection, and alarm triggers.
* **Amazon SNS:** Broadcasts automated email alerts to users during emergency thresholds.

### 4. Technical Implementation
*Implementation Stages:*
1. **Design & Modeling:** Outline database tables (Vehicle, Telemetry, Maintenance) and system architecture diagram (Weeks 1 - 4).
2. **Telemetry Pipeline Setup:** Configure AWS IoT Core, SQS, Lambda Telemetry Processor, and DynamoDB (Weeks 5 - 8).
3. **API & Dashboard Development:** Program Next.js frontend pages, S3/CloudFront hosting, Cognito authorization, and Lambda API Handler (Weeks 9 - 11).
4. **AI Integration & Testing:** Configure Gemini API for smart diagnostics, set up CloudWatch Alarms/SNS email, and perform E2E testing (Week 12).

*Technical Requirements:*
* **Simulator Endpoint:** Runs a script transmitting MQTT packets (Speed, EngineTemp, BatteryVoltage, Mileage) secured via X.509 certificates and IoT Policy.
* **Backend Infrastructure:** AWS Lambda running Node.js runtime, interacting with DynamoDB through AWS SDK.
* **Web Dashboard:** Built using Next.js, featuring responsive gauge widgets, line charts, and warning panels.
* **API Authentication:** Cognito Authorizer integration on API Gateway to validate client-side Authorization JWT headers.

### 5. Roadmap & Milestones
* **Weeks 1 - 4:** Complete requirement analysis, schema design, and AWS architecture diagram.
* **Weeks 5 - 8:** Provision AWS IoT Core pipelines, write Lambda Telemetry Processor, and deploy DynamoDB tables.
* **Weeks 9 - 11:** Complete Next.js Dashboard, integrate Cognito JWT, and configure Gemini AI endpoints.
* **Week 12 (Testing & Submission):** Configure CloudWatch/SNS alerts, perform E2E test scenarios, write report, and submit.

### 6. Budget Estimation
Below is the estimated monthly cost calculated using the AWS Pricing Calculator:

* **AWS IoT Core:** $0.08 / month (5 simulators sending messages every 10 seconds)
* **Amazon SQS:** $0.00 / month (Free tier coverage)
* **AWS Lambda:** $0.00 / month (Under 1 million requests free tier)
* **Amazon DynamoDB:** $0.25 / month (25 GB free tier, minimal read/write costs)
* **Amazon S3:** $0.15 / month (Standard storage for dashboard files)
* **Amazon CloudFront:** $0.10 / month (Global delivery traffic)
* **Amazon Cognito:** $0.00 / month (Up to 50,000 MAUs free tier)
* **Amazon API Gateway:** $0.01 / month (Minimal API calls)
* **Amazon CloudWatch & SNS:** $0.40 / month (Metrics tracking and email notifications)

**Total Monthly Estimated Cost:** $0.99 / month ($11.88 / year)

### 7. Risk Assessment
* **Network Interruptions:** If the internet connection drops, telemetry messages might fail to publish. *Mitigation:* The simulator holds data locally in a queue until the connection is restored.
* **Cognito Authorization Expiry:** User sessions could expire during active monitoring. *Mitigation:* Configure token refresh handlers on the Next.js client.
* **Cost Overruns:** Exceeding AWS Free Tier thresholds. *Mitigation:* Implement AWS Budget alerts with strict limits to stop services if they exceed $2.00/month.

### 8. Expected Outcomes
* **Real-time Monitoring:** Web dashboard displaying immediate status alerts (<2 seconds latency).
* **Smart Diagnostics:** Automated, natural-language troubleshooting advice generated by Gemini AI during critical faults.
* **Proactive Maintenance:** Clear progression visualization of maintenance cycles based on odometer calculations, preventing critical part failures.