---
title : "Deploy Amazon DynamoDB"
date : 2026-07-09
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

#### Deploy the Data Layer

In this section, you will deploy the data layer of the AutoRx system using **Amazon DynamoDB**.

The system uses Amazon DynamoDB as its NoSQL database to store vehicle information, telemetry data, maintenance records, and user information. This design provides high scalability and is well suited for real-time data processing in a Serverless architecture.

In this workshop, you will create four DynamoDB tables, each corresponding to a specific data domain in the system:

- **SmartVehicle_Vehicles**: Stores vehicle information.
- **SmartVehicle_Telemetry**: Stores time-series telemetry data.
- **SmartVehicle_Maintenance**: Stores maintenance records for each vehicle.
- **SmartVehicle_Users**: Stores user information.

![architecture](/images/5-Workshop/5.3-DynamoDB/dynamoDB-overview.jpg)

#### Contents

- [Create the SmartVehicle_Vehicles Table](5.3.1-Vehicles/)
- [Create a Global Secondary Index](5.3.2-Indexes/)
- [Create the SmartVehicle_Telemetry Table](5.3.3-Telemetry/)
- [Create the SmartVehicle_Maintenance Table](5.3.4-Maintenance/)
- [Create the SmartVehicle_Users Table](5.3.5-Users/)
- [Verify the Data Layer](5.3.6-Verify/)