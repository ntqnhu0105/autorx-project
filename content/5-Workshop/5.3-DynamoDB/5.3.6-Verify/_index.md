---
title : "Verify the Data Layer"
date : 2026-07-09
weight : 6
chapter : false
pre : " <b> 5.3.6 </b> "
---

#### Verify the DynamoDB Tables

After completing the previous steps, return to **DynamoDB → Tables**.

Make sure that all four tables have been created successfully and that their status is **Active**.

![tables](/images/5-Workshop/5.3-DynamoDB/5.3.6-Verify/tables.jpg)

Next, verify the configuration of each table:

| Table | Partition Key | Sort Key |
|--------|---------------|----------|
| SmartVehicle_Vehicles | id | - |
| SmartVehicle_Telemetry | vehicleId | timestamp |
| SmartVehicle_Maintenance | vehicleId | itemType |
| SmartVehicle_Users | id | - |

For the **SmartVehicle_Vehicles** and **SmartVehicle_Users** tables, open the **Indexes** tab and verify that the Global Secondary Index (GSI) has been created successfully.

- `licensePlate-index`

![tables](/images/5-Workshop/5.3-DynamoDB/5.3.6-Verify/5.3.6_1.jpg)

- `email-index`

![tables](/images/5-Workshop/5.3-DynamoDB/5.3.6-Verify/5.3.6_2.jpg)