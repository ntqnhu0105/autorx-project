---
title: "Workshop"
date: 2026-07-08
weight: 5
chapter: false
pre: " <b> 5. </b> "
---


# AutoRx - Smart Vehicle Monitoring and Maintenance Reminder System on AWS

#### Overview

AutoRx is a real-time vehicle health monitoring system built on a **Serverless** architecture using AWS. The system collects telemetry data from a Vehicle Simulator through the MQTT protocol, processes it with AWS Lambda, stores it in Amazon DynamoDB, and visualizes it on a web dashboard.

In addition, the system integrates multiple AWS services to ensure scalability, security, and observability:

- **AWS IoT Core** receives telemetry data from connected vehicles.
- **Amazon SQS** serves as a message queue to enable reliable and scalable data processing.
- **AWS Lambda** processes telemetry data and implements the backend APIs.
- **Amazon DynamoDB** stores vehicle telemetry data and maintenance records.
- **Amazon Cognito** provides user authentication and identity management.
- **Amazon API Gateway** exposes RESTful APIs for the web dashboard.
- **Amazon S3** and **Amazon CloudFront** host and deliver the web dashboard.
- **Amazon CloudWatch** monitors logs, metrics, and overall system performance.
- **Amazon SNS** sends email notifications when vehicle issues or maintenance reminders are triggered.

#### Contents

1. [Workshop Overview](5.1-Workshop-overview/)
2. [Prerequisites](5.2-Prerequisite/)
3. [Deploy Amazon DynamoDB](5.3-DynamoDB/)
4. [Build the IoT Telemetry Pipeline](5.4-IoT-Pipeline/)
5. [Deploy the Backend API](5.5-Backend-API/)
6. [Configure Authentication](5.6-Authentication/)
7. [Deploy the Web Dashboard](5.7-Web-Dashboard/)
8. [Set Up Monitoring and Notifications](5.8-Monitoring/)
9. [Test the System](5.9-Testing/)
10. [Clean Up Resources](5.10-Cleanup/)