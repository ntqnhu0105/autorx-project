---
title: "Telemetry Flow (IoT Core & Lambda)"
date: 2026-07-09
weight: 3
chapter: false
pre: " <b> 5.3 </b> "
---

# Setting up the Telemetry Data Flow

#### 1. Configure AWS IoT Core
* Create a **Thing** in AWS IoT Core representing the smart vehicle (e.g., `SmartVehicle_01`).
* Download the **Certificates** (Public key, Private key, Device certificate) to allow the simulated device to securely authenticate and connect with AWS IoT.
* Create and attach an **IoT Policy** to the Certificate to grant the device permissions to Publish and Subscribe to specific MQTT topics (e.g., `telemetry/vehicle/#`).

![policy](/images/5-Workshop/5.3-Telemetry-flow/result.jpg)

#### 2. Create AWS Lambda Function for data processing
* Go to the AWS Lambda service and create a new function (Runtime: Node.js or Python).
![function](/images/5-Workshop/5.3-Telemetry-flow/create-function.jpg)

* This Lambda function will be responsible for: 
  - Parsing the JSON string containing data sent from the device.
  - Checking critical indicators (engine temperature, speed).
  - Calculating mileage to determine if the vehicle is due for maintenance.
* Assign an **IAM Role** to the Lambda to allow logging to CloudWatch and interacting with DynamoDB in the next steps.

#### 3. Configure IoT Rule (Message Routing)
* In AWS IoT Core, create a **Message Routing Rule** to automatically forward data received from the MQTT Topic to the newly created Lambda Function.
* Write an SQL statement to query the data (e.g., `SELECT * FROM 'telemetry/vehicle/+'`).
![sql](/images/5-Workshop/5.3-Telemetry-flow/sql.jpg)

* Configure the **Action** for the Rule to invoke the target Lambda function, enabling a fully automated real-time data processing workflow.

![action](/images/5-Workshop/5.3-Telemetry-flow/rule-actions.jpg)
