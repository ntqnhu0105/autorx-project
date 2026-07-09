---
title : "Kiểm tra Amazon SQS Queue"
date : 2026-01-01
weight : 3
chapter : false
pre : " <b> 5.4.2.3 </b> "
---

Sau khi tạo thành công Queue, chúng ta sẽ kiểm tra các thông tin cơ bản của Amazon SQS.

---

### Kiểm tra Queue

Truy cập:

```
Amazon SQS

↓

Queues

↓

AutoRxTelemetryQueue
```

Kiểm tra trạng thái Queue.

---

### Kiểm tra ARN

Trong phần **Details**, ghi nhận giá trị:

```
Queue ARN
```

ARN này sẽ được sử dụng khi cấu hình **AWS IoT Rule** ở bước tiếp theo.


---

![queue-detail](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/verify/queue-detail.jpg)


Sau khi hoàn thành bước này, Amazon SQS Queue đã sẵn sàng để nhận dữ liệu Telemetry từ AWS IoT Core thông qua AWS IoT Rule.