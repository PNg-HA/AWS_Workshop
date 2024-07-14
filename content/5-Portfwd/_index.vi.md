---
title : "Port Forwarding"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

{{% notice info %}}
**Port Forwarding** là mốt cách thức hữu ích để chuyển hướng lưu lượng mạng từ 1 địa chỉ IP - Port này sang 1 địa chỉ IP - Port khác. Với **Port Forwarding** chúng ta có thể truy cập một EC2 instance nằm trong private subnet từ máy trạm của chúng ta.
{{% /notice %}}

Chúng ta sẽ cấu hình **Port Forwarding** cho kết nối RDP giữa máy của mình với **Private Windows Instance** nằm trong private subnet mà chúng ta đã tạo cho bài thực hành này.

![port-fwd](/images/arc-04.png) 



#### Tạo IAM User có quyền kết nối SSM

1. Truy cập vào [giao diện quản trị dịch vụ IAM](https://console.aws.amazon.com/iamv2/home)
  + Click **Users** , sau đó click **Add users**.

![FWD](/images/5.fwd/001-fwd.png)

2. Tại trang **Add user**.
  + Tại mục **User name**, điền **Portfwd**.
  + Click chọn **Access key - Programmatic access**.
  + Click **Next: Permissions**.
  
![FWD](/images/5.fwd/002-fwd.png)

3. Click **Attach existing policies directly**.
  + Tại ô tìm kiếm , điền **ssm**.
  + Click chọn **AmazonSSMFullAccess**.
  + Click **Next: Tags**, click **Next: Reviews**.
  + Click **Create user**.

4. Lưu lại thông tin **Access key ID** và **Secret access key** để thực hiện cấu hình AWS CLI.

#### Cài đặt và cấu hình AWS CLI và Session Manager Plugin 
  
Để thực hiện phần thực hành này, đảm bảo máy trạm của bạn đã cài [AWS CLI]() và [Session Manager Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-install-plugin.html)

Bạn có thể tham khảo thêm bài thực hành về cài đặt và cấu hình AWS CLI [tại đây](https://000011.awsstudygroup.com/).

{{%notice tip%}}
Với Windows thì khi giải nén thư mục cài đặt **Session Manager Plugin** bạn hãy chạy file **install.bat** với quyền Administrator để thực hiện cài đặt.
{{%/notice%}}

#### Thực hiện Portforwarding 

1. Chạy command dưới đây trong **Command Prompt** trên máy của bạn để cấu hình **Port Forwarding**.

```
  aws ssm start-session --target (your ID windows instance) --document-name AWS-StartPortForwardingSession --parameters portNumber="3389",localPortNumber="9999" --region (your region) 
```
{{%notice tip%}}

Thông tin **Instance ID** của **Windows Private Instance** có thể tìm được khi bạn xem chi tiết máy chủ EC2 Windows Private Instance.

{{%/notice%}}

  + Câu lệnh ví dụ

```
C:\Windows\system32>aws ssm start-session --target i-06343d7377486760c --document-name AWS-StartPortForwardingSession --parameters portNumber="3389",localPortNumber="9999" --region ap-southeast-1
```

{{%notice warning%}}

Nếu câu lệnh của bạn báo lỗi như dưới đây : \
SessionManagerPlugin is not found. Please refer to SessionManager Documentation here: http://docs.aws.amazon.com/console/systems-manager/session-manager-plugin-not-found\
Chứng tỏ bạn chưa cài Session Manager Plugin thành công. Bạn có thể cần khởi chạy lại **Command Prompt** sau khi cài **Session Manager Plugin**.

{{%/notice%}}

2. Kết nối tới **Private Windows Instance** bạn đã tạo bằng công cụ **Remote Desktop** trên máy trạm của bạn.
  + Tại mục Computer: điền **localhost:9999**.


![FWD](/images/5.fwd/003-fwd.png)


3. Quay trở lại giao diện quản trị của dịch vụ System Manager - Session Manager.
  + Click tab **Session history**.
  + Chúng ta sẽ thấy các session logs với tên Document là **AWS-StartPortForwardingSession**.


![FWD](/images/5.fwd/004-fwd.png)


Chúc mừng bạn đã hoàn tất bài thực hành hướng dẫn cách sử dụng Session Manager để kết nối cũng như lưu trữ các session logs trong S3 bucket. Hãy nhớ thực hiện bước dọn dẹp tài nguyên để tránh sinh chi phí ngoài ý muốn nhé.