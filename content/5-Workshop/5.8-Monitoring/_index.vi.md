---
title : "Thiết lập Monitoring và Notification"
date : 2026-07-09
weight : 8
chapter : false
pre : " <b> 5.8. </b> "
---

#### Tổng quan

Trong phần này, bạn sẽ cấu hình **giám sát và cảnh báo** cho hệ thống AutoRx bằng **Amazon CloudWatch** và **Amazon SNS**.

CloudWatch thu thập log và metric từ các Lambda function. SNS gửi email cảnh báo khi alarm kích hoạt hoặc khi Telemetry Processor phát hiện dữ liệu xe bất thường (ví dụ: nhiệt độ động cơ cao, điện áp pin thấp).

![Monitoring Architecture](/images/5-Workshop/5.8-Monitoring/monitoring-overview.png)

---

## 5.8.1 — Tạo SNS Topic và Subscription

### Tạo topic

1. Mở [SNS Console](https://console.aws.amazon.com/sns)
2. Nhấn **Topics** → **Create topic**
3. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Type | `Standard` |
| Name | `autorx-alerts` |

4. Nhấn **Create topic**

### Thêm email subscription

1. Mở `autorx-alerts` → nhấn **Create subscription**
2. Điền thông tin:

| Trường | Giá trị |
|--------|---------|
| Protocol | `Email` |
| Endpoint | Địa chỉ email của bạn |

3. Nhấn **Create subscription**
4. Kiểm tra hộp thư và nhấn **Confirm subscription** trong email xác nhận

![sns-topic](/images/5-Workshop/5.8-Monitoring/sns-topic.png)

---

## 5.8.2 — Xem CloudWatch Logs của Lambda

Lambda tự động gửi log thực thi vào CloudWatch. Dùng log để theo dõi hoạt động và debug lỗi.

### Xem log

1. Mở [CloudWatch Console](https://console.aws.amazon.com/cloudwatch)
2. Vào **Logs → Log groups**
3. Tìm và mở:
   - `/aws/lambda/AutoRxAPIHandler`
   - `/aws/lambda/AutoRxTelemetryProcessor`
4. Nhấn vào **Log stream** để xem log thực thi gần nhất

### Những gì cần chú ý

- Dòng `START` / `END` / `REPORT` xác nhận function được gọi thành công
- Dòng lỗi (`ERROR`, `Exception`) là các vấn đề cần điều tra
- Dòng `REPORT` hiển thị thời gian tính phí và mức sử dụng bộ nhớ

![cloudwatch-logs](/images/5-Workshop/5.8-Monitoring/cloudwatch-logs.png)

---

## 5.8.3 — Tạo CloudWatch Alarms

Tạo alarm để tự động thông báo khi lỗi Lambda vượt ngưỡng cho phép.

### Alarm: Lỗi API Handler

1. Trong CloudWatch, vào **Alarms** → **Create alarm**
2. Nhấn **Select metric** → **Lambda** → **By Function Name**
3. Chọn `AutoRxAPIHandler` → metric `Errors`
4. Cấu hình:

| Cài đặt | Giá trị |
|---------|---------|
| Statistic | `Sum` |
| Period | `5 minutes` |
| Threshold type | `Static` |
| Condition | `Greater than` `0` |

5. Ở **Notification**, chọn **In alarm** → chọn SNS topic `autorx-alerts`
6. Đặt tên alarm: `AutoRxAPIHandler-Errors`
7. Nhấn **Create alarm**

Lặp lại tương tự cho `AutoRxTelemetryProcessor` → tên alarm: `AutoRxTelemetryProcessor-Errors`.

![cloudwatch-alarm](/images/5-Workshop/5.8-Monitoring/cloudwatch-alarm.png)

---

## 5.8.4 — Kiểm tra thông báo

### Kích hoạt alarm thủ công

Buộc lỗi Lambda hoặc dùng lệnh CLI để đặt trạng thái alarm:

```bash
aws cloudwatch set-alarm-state \
  --alarm-name AutoRxAPIHandler-Errors \
  --state-value ALARM \
  --state-reason "Manual test" \
  --region ap-southeast-1
```

Kiểm tra hộp thư — email cảnh báo từ SNS sẽ đến trong vòng 1–2 phút.

### Reset alarm

```bash
aws cloudwatch set-alarm-state \
  --alarm-name AutoRxAPIHandler-Errors \
  --state-value OK \
  --state-reason "Test complete" \
  --region ap-southeast-1
```

![verify-alarm](/images/5-Workshop/5.8-Monitoring/verify-alarm.png)

---

### Kết quả

CloudWatch alarm đã hoạt động và kết nối với SNS topic `autorx-alerts`. Mọi lỗi Lambda trong hệ thống AutoRx sẽ tự động kích hoạt email thông báo.
