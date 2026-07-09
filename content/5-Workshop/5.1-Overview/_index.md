---
title: "Workshop Overview"
date: 2026-07-09
weight: 1
chapter: false
pre: " <b> 5.1 </b> "
---

# Workshop Overview

#### 1. Context & Problem Statement
Currently, manual vehicle management (for personal use, delivery fleets, or service vehicles) faces significant limitations. Vehicle owners often forget periodic maintenance schedules, and it is difficult to closely monitor the wear and tear of critical components. This can lead to severe on-road breakdowns and safety hazards. Furthermore, the lack of an operational history database makes it challenging for transportation businesses to optimize costs and vehicle lifespan.

To solve this problem, we need a system capable of automatically and continuously collecting telemetry data from vehicles and processing it intelligently to issue timely alerts.

#### 2. Workshop Objectives
Through this workshop, you will gain hands-on experience building a complete **Smart Vehicle Management** system leveraging cloud services on the AWS platform. By the end of this workshop, you will understand:
* How to connect IoT devices to the cloud using the MQTT protocol.
* Real-time data processing skills using Serverless architecture.
* How to design a NoSQL database optimized for vehicle data streams.
* The ability to set up monitoring systems and automatically trigger smart alerts via Email/SMS.

#### 3. System Architecture

![architect](/images/5-Workshop/5.1-Workshop-overview/architect.jpg)

The project's architecture is designed using an **Event-Driven** and **Serverless** model. It is divided into two independent yet tightly integrated processing flows:

* **Telemetry Flow (For vehicles):**
  - On-board devices (or the Vehicle Simulator) continuously collect data (speed, engine temperature, mileage) and send it directly to **AWS IoT Core** via MQTT.
  - **IoT Rule** acts as an orchestrator, automatically filtering and forwarding these critical messages to **AWS Lambda** for safety threshold analysis.
  - After processing, the data is permanently stored in **Amazon DynamoDB**.

* **Management Flow (For users/Dashboard):**
  - Users or vehicle owners looking up maintenance information will interact with a web application (Frontend).
  - This application sends HTTP requests to **Amazon API Gateway**.
  - API Gateway routes these requests to an independent **AWS Lambda** function, dedicated to reading/writing data from **DynamoDB** and returning results to the user.

* **Monitoring & Alerting System:**
  - All system activities are logged in **CloudWatch Logs** for easy debugging.
  - When an anomaly is detected (e.g., engine temperature exceeds 105°C), a **CloudWatch Alarm** is instantly triggered, prompting **Amazon SNS** to send an emergency alert message to the vehicle owner's device.

In the upcoming sections, we will dive into the practical implementation of each component!

