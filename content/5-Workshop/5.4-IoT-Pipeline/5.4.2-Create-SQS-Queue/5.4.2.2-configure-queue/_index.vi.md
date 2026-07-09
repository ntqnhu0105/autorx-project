---
title : "Cấu hình Queue"
date : 2026-07-09
weight : 2
chapter : false
pre : " <b> 5.4.2.2 </b> "
---

Sau khi đặt tên Queue, chúng ta sẽ cấu hình các thông số cho Amazon SQS.

Đối với workshop này, có thể giữ nguyên hầu hết các giá trị mặc định.

---

### Visibility Timeout

Thiết lập:

```
30 Seconds
```

Khoảng thời gian này giúp tránh nhiều Lambda xử lý cùng một bản tin.

![visibility](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/configure-queue/visibility.jpg)

---

### Message Retention

Giữ nguyên:

```
4 Days
```

Khoảng thời gian này cho phép hệ thống lưu bản tin trong trường hợp quá trình xử lý bị gián đoạn.

![retention](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/configure-queue/retention.jpg)

---

### Encryption

Chọn:

```
Server-side encryption

↓

SSE-SQS
```

Amazon SQS sẽ tự động mã hóa dữ liệu lưu trong Queue.

![encryption](/images/5-Workshop/5.4-IoT-Pipeline/5.4.2-create-sqs/configure-queue/encryption.jpg)

---

### Hoàn tất

Giữ nguyên các thiết lập còn lại.

Chọn **Create Queue**.
