---
title : "Introduction"
date : 2026-07-08 
weight : 1 
chapter : false
pre : " <b> 5.1. </b> "
---

#### Introduction to AutoRx

AutoRx is a real-time vehicle health monitoring system built on a serverless architecture using AWS. The system collects telemetry data from a Vehicle Simulator through the MQTT protocol, processes it using AWS Lambda, and stores it in Amazon DynamoDB. Users can monitor vehicle status through a web dashboard and receive notifications when a vehicle encounters an issue or reaches its scheduled maintenance interval.

The system is designed with the following objectives:

- Collect real-time telemetry data from connected vehicles.
- Monitor vehicle health and operating status.
- Automatically generate maintenance reminders based on the odometer (ODO) reading.
- Provide an intuitive web dashboard for vehicle monitoring.
- Send email notifications when abnormal conditions are detected.
- Leverage a serverless architecture to reduce operational costs and improve scalability.

---

#### Workshop Overview

In this workshop, you will deploy the complete AutoRx solution on AWS Cloud using a serverless architecture.

The system consists of two main workflows:

1. **Telemetry Pipeline**
   - The Vehicle Simulator acts as an IoT device and publishes telemetry data to **AWS IoT Core** using the MQTT protocol with **X.509 certificates** for secure authentication.
   - After receiving the telemetry data, the **AWS IoT Rule Engine** forwards the messages to **Amazon SQS**, ensuring reliable and scalable processing during traffic spikes.
   - Messages in the queue are processed by **AWS Lambda (Telemetry Processor)** and stored in **Amazon DynamoDB**. At the same time, Lambda writes logs to **Amazon CloudWatch** and triggers **Amazon SNS** to send email alerts whenever predefined thresholds are exceeded.

2. **Web Dashboard**
   - Users access the dashboard through **Amazon CloudFront**, while the website's static content is hosted on **Amazon S3**.
   - To access system data, users authenticate with **Amazon Cognito**, which issues a JWT access token.
   - The JWT token is included in requests sent to **Amazon API Gateway**. After successful authentication, API Gateway invokes **AWS Lambda (API Handler)** to retrieve data from **Amazon DynamoDB** and return the results to the dashboard.

---

#### System Architecture

The following diagram illustrates the overall architecture of the AutoRx system.

![overview](/images/5-Workshop/5.1-Workshop-overview/architect.jpg)

---

#### AWS Services Used

This workshop uses the following AWS services:

- **AWS IoT Core** receives telemetry data from the Vehicle Simulator through the MQTT protocol.
- **AWS IoT Rule Engine** routes telemetry messages from AWS IoT Core to Amazon SQS.
- **Amazon SQS** serves as a message queue to ensure reliable and scalable data processing.
- **AWS Lambda** processes telemetry data and implements the backend APIs.
- **Amazon DynamoDB** stores telemetry data, vehicle status, and maintenance records.
- **Amazon Cognito** manages user authentication and issues JWT tokens.
- **Amazon API Gateway** provides REST APIs for the web dashboard.
- **Amazon S3** hosts the static files of the web dashboard.
- **Amazon CloudFront** delivers the dashboard through a global CDN for improved performance.
- **Amazon CloudWatch** collects logs, monitors system performance, and generates operational metrics.
- **Amazon SNS** sends email notifications when vehicle issues or maintenance reminders are triggered.

---

#### System Workflow

The AutoRx system operates as follows:

1. The Vehicle Simulator publishes telemetry data to AWS IoT Core using the MQTT protocol.
2. The AWS IoT Rule Engine forwards the telemetry messages to Amazon SQS.
3. AWS Lambda retrieves messages from SQS and stores the processed data in Amazon DynamoDB.
4. Lambda writes execution logs to Amazon CloudWatch and evaluates alert conditions.
5. When abnormal conditions are detected, Amazon SNS sends email notifications to subscribed users.
6. Users authenticate through Amazon Cognito and receive a JWT access token.
7. The web dashboard sends API requests to Amazon API Gateway with the JWT token.
8. API Gateway validates the JWT token and invokes AWS Lambda.
9. Lambda queries Amazon DynamoDB and returns the requested data to the web dashboard.