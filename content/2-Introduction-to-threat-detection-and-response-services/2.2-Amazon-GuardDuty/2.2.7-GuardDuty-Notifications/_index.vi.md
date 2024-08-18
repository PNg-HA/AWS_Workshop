---
title : "GuardDuty - Thông báo"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 2.2.7 </b> "
---

#### Configure SNS topic

1. Điều hướng đến Amazon SNS. https://console.aws.amazon.com/sns/v3/home 

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s1.png)
2. Chọn **Topics** ở thanh điều hướng bên trái.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s2.png)

3. Chọn **Create topic**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s3.png)

1. Chọn **Standard**.


5. Nhập **"guardduty-findings"** cho tên.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s5.png)

6. Để lại mọi thứ còn lại mặc định và chọn **Create topic** ở dưới cùng của trang. Điều này sẽ tạo ra topic.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s6.png)
#### Đăng ký theo dõi topic

1. Từ trang **guardduty-findings** topic, chọn **Create subscription**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s7.png)

1. Trong trang **Create subscription**, dưới **Protocol**, chọn **email**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s8.png)

1. Trong trang **Create subscription**, dưới **Endpoint**, nhập địa chỉ email mà bạn muốn sử dụng cho workshop này để nhận thông báo. Bạn có thể hủy đăng ký vào cuối workshop.


10. Chọn **Create subscription**. 
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s10a.png)

11. Kiểm tra email bạn đã nhập. Trong vài phút, bạn sẽ nhận được một email
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s11.png)

12. Xác nhận đăng ký bằng cách chọn "Confirm subscription" trong email. Điều này sẽ mở một trang web xác nhận.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s12.png)

#### Tạo EventBridge Rule để gửi phát hiện đến topic

13. Bây giờ bạn đã đăng ký theo dõi SNS topic, bạn sẵn sàng gửi các phát hiện đến đó. Tạo một EventBridge rule để lắng nghe các sự kiện từ Security Hub. Điều hướng đến Amazon EventBridge. https://console.aws.amazon.com/events/home
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s13.png)

14. Chọn nút **Create rule** ở bên phải với "EventBridge Rule" được chọn.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s14.png)

1.  Trong trang **Define rule detail**, đặt tên cho rule của bạn là "guardduty-findings". Chọn **Next**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s15.png)

1.  Trong trang **Build event pattern**, cuộn xuống phần **Event pattern**, chọn **Edit pattern**  ở góc dưới bên phải.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s16.png)

1.  Thêm mẫu sự kiện sau (JSON). Mẫu này sẽ xác định các sự kiện cho các phát hiện của Security Hub được gán mức độ nghiêm trọng CRITICAL.

```js
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"],
  "detail": {
    "severity": [
      4, 4.0, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 4.8, 4.9, 
      5, 5.0, 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8, 5.9, 
      6, 6.0, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 
      7, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 7.8, 7.9, 
      8, 8.0, 8.1, 8.2, 8.3, 8.4, 8.5, 8.6, 8.7, 8.8, 8.9
      ]
  }
}
```
18. Chọn **Next**.


19. Trong trang **Select target(s)**, từ menu thả xuống **Select a target**, chọn **SNS topic**.


20. Sau đó, từ menu thả xuống **Topic**, chọn **guardduty-findings**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s20.png)

21.  Chọn **Next**.


22.  Trong trang **Configure tags - optional**, chọn **Next**.


23.  Trong trang **Review and create**, chọn **Create rule**. Hãy chú ý đến email của bạn trong phần còn lại của workshop.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s23.png)
