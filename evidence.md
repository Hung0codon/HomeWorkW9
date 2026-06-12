# Báo cáo Minh chứng Thực hành (Lab Evidence) - CloudWatch & SNS

Tài liệu này tổng hợp hình ảnh minh chứng kết quả cài đặt CloudWatch Agent trên máy ảo EC2 và thiết lập cảnh báo CPU Alarm gửi email thông qua Amazon SNS.

---

## 1. Cài đặt CloudWatch Agent trên EC2
Để thu thập các chỉ số chi tiết từ hệ điều hành của máy ảo EC2 (như bộ nhớ RAM, dung lượng đĩa cứng, CPU chi tiết), CloudWatch Agent đã được cài đặt và cấu hình thành công trên máy ảo.

### Thông tin máy ảo EC2 thực tế:
* **Tên Instance:** `quochung-monitored-instance`
* **Instance ID:** `i-0bc43322e70534c9f`
* **Địa chỉ Public IP:** `13.213.51.174`
* **Địa chỉ Private IP:** `10.0.1.9`

### Kiểm tra trạng thái hoạt động của Agent:
Trạng thái hoạt động của CloudWatch Agent được kiểm tra bằng lệnh:
```bash
sudo /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -m ec2 -a status
```

**Kết quả đầu ra:**
```json
{
  "status": "running",
  "starttime": "2026-06-12T10:03:50+00:00",
  "configstatus": "configured",
  "version": "1.300066.2"
}
```

### Hình ảnh minh chứng:
**Trạng thái CloudWatch Agent**

<img width="1215" height="618" alt="image" src="https://github.com/user-attachments/assets/a22f4751-2219-4f9c-8c1d-7f5791b51e8b" />


---

## 2. Cấu hình CPU Alarm gửi Email Alert qua SNS
Thiết lập cảnh báo CloudWatch Alarm giám sát chỉ số sử dụng CPU (`CPUUtilization`) của máy ảo EC2, tự động gửi thông báo qua email khi CPU vượt ngưỡng cấu hình thông qua Amazon SNS (Simple Notification Service) về hòm thư **`quochungisme@gmail.com`**.

### Chi tiết cấu hình Alarm thực tế:
* **Tên Alarm:** `ec2-i-0bc43322e70534c9f-high-cpu-alarm`
* **Namespace:** `AWS/EC2`
* **Chỉ số giám sát (Metric name):** `CPUUtilization`
* **Instance ID:** `i-0bc43322e70534c9f`
* **Ngưỡng cảnh báo (Threshold):** `CPUUtilization > 80` trong vòng 1 điểm dữ liệu liên tiếp 5 phút (for 1 datapoints within 5 minutes).
* **Hành động (Actions):** Đã kích hoạt (Actions enabled), liên kết với SNS Topic `arn:aws:sns:ap-southeast-1:945125812908:ec2-cpu-alerts-topic` để gửi email tới **`quochungisme@gmail.com`**.
* **Trạng thái hiện tại:** `Insufficient data` / `OK` (Đang thu thập dữ liệu ban đầu).
* **Alarm ARN:** `arn:aws:cloudwatch:ap-southeast-1:945125812908:alarm:ec2-i-0bc43322e70534c9f-high-cpu-alarm`

### Hình ảnh minh chứng:
**Cấu hình CPU Alarm trên CloudWatch**

<img width="1550" height="694" alt="image" src="https://github.com/user-attachments/assets/234adec0-cdbd-44f3-b307-b9dbb3a84eed" />

