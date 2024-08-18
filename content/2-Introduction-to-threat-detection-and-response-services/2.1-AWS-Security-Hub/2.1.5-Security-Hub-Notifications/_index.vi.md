---
title : "Security Hub - Thông báo"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.1.5 </b> "
---

#### Cấu hình SNS topic

1. Truy cập Amazon SNS. https://console.aws.amazon.com/sns/v3/home 

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s1.png)
2. Chọn **Topics** trong thanh điều hướng bên trái.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s2.png)

3. Chọn **Create topic**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s3.png)

4. Chọn loại  **Standard**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s4.png)

5. Đối với tên, nhập **"security-hub-findings"**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s5.png)

6. Để mọi thứ mặc định và chọn **Create topic** ở cuối trang. Điều này sẽ tạo topic.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s6.png)
#### Đăng ký topic

7. Từ trang **security-hub-findings** topic, chọn **Create subscription**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s7.png)

8. Trong trang **Create subscription**, dưới mục **Protocol**, chọn **email**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s8.png)

9. Trong trang **Create subscription**, dưới mục **Endpoint**, nhập địa chỉ email mà bạn muốn sử dụng cho workshop này để nhận thông báo. Bạn có thể hủy đăng ký vào cuối workshop.

10.  Chọn **Create subscription**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s10.png)

11.  Kiểm tra email bạn đã nhập. Trong vòng vài phút, bạn sẽ nhận được một email đến email đó.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s11.png)

12.  Xác nhận đăng ký bằng cách chọn "Confirm subscription" trong email. Nó sẽ mở ra một trang web xác nhận.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s12.png)

#### Tạo EventBridge Rule để gửi các phát hiện đến topic

13. Bây giờ bạn đã đăng ký SNS topic, bạn đã sẵn sàng để gửi các phát hiện đến đó. Tạo một EventBridge rule  để lắng nghe các sự kiện từ Security Hub. Điều hướng đến Amazon EventBridge. https://console.aws.amazon.com/events/home
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s13.png)

14. Chọn **Create rule** ở bên phải "EventBridge Rule" được chọn.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s14.png)

15.  Trên trang **Define rule detail**, đặt tên cho rule là "security-hub-findings". Chọn **Next**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s15.png)

16.  Trên trang **Build event pattern**, cuộn xuống phần **Event pattern**, chọn **Edit pattern** ở góc dưới bên phải.


17. Thêm mẫu sự kiện (JSON) sau đây. Mẫu này sẽ xác định các sự kiện cho các phát hiện của Security Hub được gắn nhãn mức độ nghiêm trọng CRITICAL.

```js
{
  "source": [
    "aws.securityhub"
  ],
  "detail-type": [
    "Security Hub Findings - Imported"
  ],
  "detail": {
    "findings": {
      "ProductName": [
        "Security Hub"
      ],
      "Severity": {
        "Label": [
          "CRITICAL"
        ]
      }
    }
  }
}
```
18. Chọn **Next**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s18.png)

19.  Trong trang **Select target(s)**, từ menu thả xuống **Select a target**, chọn **SNS topic**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s19.png)

20.  Sau đó, từ menu thả xuống **Topic**, chọn **security-hub-findings**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s20.png)

21.  Chọn **Next**.


22.  Trong trang **Configure tags - optional**, chọn **Next**.


23.  Trong trang **Review and create**, chọn **Create rule**. Theo dõi email của bạn trong suốt phần còn lại của workshop.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s23.png)

Xem rule đã được tạo:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s24.png)