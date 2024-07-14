---
title : "Kết nối đến máy chủ Public"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 3.1. </b> "
---
![SSMPublicinstance](/images/arc-02.png)

1. Truy cập vào [giao diện quản trị của dịch vụ EC2](https://console.aws.amazon.com/ec2/v2/home).
  + Click chọn **Public Linux Instance**.
  + Click **Actions**.
  + Click **Security**.
  + Click **Modify IAM role**.

![Connect](/images/3.connect/001-connect.png)

2. Tại trang Modify IAM role.
  + Click chọn **SSM-Role**.
  + Click **Save**.

{{% notice note %}}
Bạn sẽ cần chờ khoảng 10 phút trước khi thực hiện bước tiếp theo. Thời gian này EC2 instance của chúng ta sẽ tự động đăng ký với Session Manager.
{{% /notice %}}

3. Truy cập vào [giao diện quản trị của dịch vụ AWS Systems Manager](https://console.aws.amazon.com/systems-manager/home)
  + Kéo thanh trượt menu bên trái xuống dưới.
  + Click **Session Manager**.
  + Click **Start Session**.


![Connect](/images/3.connect/002-connect.png)


4. Sau đó chọn **Public Linux Instance** và click **Start session** để truy cập vào instance.

![Connect](/images/3.connect/003-connect.png)


5. Terminal sẽ xuất hiện trên trình duyệt. Kiểm tra với câu lệnh ``` sudo tcpdump -nn port 22 ``` và ```sudo tcpdump ``` chúng ta sẽ thấy không có traffic của SSH mà chỉ có traffic HTTPS.

![Connect](/images/3.connect/004-connect.png)

{{% notice note %}}
 Ở trên, chúng ta đã tạo  kết nối vào public instance mà không cần mở cổng SSH 22, giúp cho việc bảo mật tốt hơn, tránh mọi sự tấn công tới cổng SSH.\
Một nhược điểm của cách làm trên là ta phải mở Security Group outbound ở cổng 443 ra ngoài internet. Vì là public instance nên có thể sẽ không vấn đề gì nhưng nếu bạn muốn bảo mật hơn nữa, bạn có thể khoá cổng 443 ra ngoài internet mà vẫn sử dụng được Session Manager. Chúng ta sẽ đi qua cách làm này ở phần private instance dưới đây.
 {{% /notice %}}

 Bạn có thể terminate để kết thúc session đang kết nối trước khi qua bước tiếp theo.

