---
title : "Kết nối EC2 Private"
date :  "`r Sys.Date()`" 
weight : 3 
chapter : false
pre : " <b> 3.2.3 </b> "
---


#### Gán IAM role và restart EC2 instance.

1. Truy cập [giao diện quản trị dịch vụ EC2](https://console.aws.amazon.com/ec2/v2/home)
  + Click chọn **Private Windows Instance**.
  + Click **Actions**.
  + Click **Security**.
  + Click **Modify IAM Role**.

![Connect](/images/3.connect/027-ec2role.png)

2. Tại trang **Modify IAM Role**.
  + Tại mục IAM role, chọn **SSM-Role**.
  + Click **Save**.

![Connect](/images/3.connect/028-ec2role.png)

3. Click chọn **Private Windows Instance**.
  + Click **Instance state**.
  + Click **Reboot instance** để thực hiện restart, sau đó click **Reboot** để xác nhận.

![Connect](/images/3.connect/029-ec2role.png)

{{% notice note %}}
Bạn hãy đợi 5 phút trước khi làm bước tiếp theo nhé.
 {{% /notice %}}

#### Kết nối tới máy chủ private EC2 instance.

1. Truy cập vào [giao diện quản trị dịch vụ System Manager - Session Manager](https://console.aws.amazon.com/systems-manager/session-manager)
  + Click **Start session**.
  
2. Click chọn **Private Windows Instance**.
  + Click **Start session**.

3. Gõ lệnh **ipconfig** để kiểm tra thông tin địa chỉ IP  của **Private Windows Instance** như dưới đây.

![Connect](/images/3.connect/030-ec2role.png)
