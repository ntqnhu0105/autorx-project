---
title : "Kiểm tra Data Layer"
date : 2026-07-09
weight : 6
chapter : false
pre : " <b> 5.3.6 </b> "
---

#### Kiểm tra các bảng DynamoDB

Sau khi hoàn thành các bước trên, quay lại **DynamoDB → Tables**.

Đảm bảo rằng cả bốn bảng đều đã được tạo thành công và ở trạng thái **Active**.

![tables](/images/5-Workshop/5.3-DynamoDB/5.3.6-Verify/tables.jpg)

Tiếp theo, kiểm tra cấu hình của từng bảng:

| Table | Partition Key | Sort Key |
|--------|---------------|----------|
| SmartVehicle_Vehicles | id | - |
| SmartVehicle_Telemetry | vehicleId | timestamp |
| SmartVehicle_Maintenance | vehicleId | itemType |
| SmartVehicle_Users | id | - |

Đối với hai bảng **SmartVehicle_Vehicles** và **SmartVehicle_Users**, hãy kiểm tra tab **Indexes** để xác nhận Global Secondary Index (GSI) đã được tạo thành công.

- `licensePlate-index`
![tables](/images/5-Workshop/5.3-DynamoDB/5.3.6-Verify/5.3.6_1.jpg)

- `email-index`
![tables](/images/5-Workshop/5.3-DynamoDB/5.3.6-Verify/5.3.6_2.jpg)
