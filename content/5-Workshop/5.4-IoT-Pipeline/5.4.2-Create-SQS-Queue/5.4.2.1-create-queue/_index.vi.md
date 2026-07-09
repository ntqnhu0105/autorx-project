---
title : "Tạo Amazon SQS Queue"
date : 2026-07-09
weight : 1
chapter : false
pre : " <b> 5.4.2.1 </b> "
---

Trong bước này, chúng ta sẽ tạo một **Amazon SQS Standard Queue** để lưu trữ các bản tin Telemetry được chuyển tiếp từ AWS IoT Core.

Amazon SQS sẽ đóng vai trò là hàng đợi trung gian, giúp tách biệt quá trình tiếp nhận dữ liệu và xử lý dữ liệu.

---

### Truy cập Amazon SQS

Từ **AWS Management Console**, tìm kiếm dịch vụ:

```
Amazon SQS
```

Chọn **Create queue**.

![sqs-home](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/create-queue/sqs-home.jpg)

---

### Chọn loại Queue

Chọn:

```
Standard
```

Đây là loại Queue phù hợp với hệ thống Telemetry nhờ khả năng xử lý thông lượng lớn và chi phí thấp.

![queue-type](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/create-queue/queue-type.jpg)

---

### Đặt tên Queue

Nhập tên:

```
AutoRxTelemetryQueue
```

Sau đó chọn **Next**.

![queue-name](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/create-queue/queue-name.jpg)