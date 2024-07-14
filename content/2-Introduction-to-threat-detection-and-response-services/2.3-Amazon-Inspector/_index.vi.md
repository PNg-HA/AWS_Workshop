---
title : "Tạo IAM Role"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2.2 </b> "
---

### Tạo IAM Role

Trong bước này chúng ta sẽ tiến hành tạo IAM Role. Trong IAM Role này sẽ được gán policy **AmazonSSMManagedInstanceCore**, đây là policy cho phép máy chủ EC2 có thể giao tiếp với Session Manager.

1. Truy cập vào [giao diện quản trị dịch vụ IAM](https://console.aws.amazon.com/iamv2/)
2. Ở thanh điều hướng bên trái, click  **Roles**.  

![role](/images/2.prerequisite/038-iamrole.png)

3. Click **Create role**.  

![role1](/images/2.prerequisite/039-iamrole.png)

4. Click **AWS service** và click **EC2**. 
  + Click **Next: Permissions**.  

![role1](/images/2.prerequisite/040-iamrole.png)

5. Trong ô Search, điền **AmazonSSMManagedInstanceCore** và ấn phím Enter để tìm kiếm policy này.
  + Click chọn policy **AmazonSSMManagedInstanceCore**.
  + Click **Next: Tags.**

![createpolicy](/images/2.prerequisite/041-iamrole.png)

6. Click **Next: Review**.
7. Đặt tên cho Role là **SSM-Role** ở Role Name  
  + Click **Create Role** \.

![namerole](/images/2.prerequisite/042-iamrole.png)

Tiếp theo chúng ta sẽ thực hiện kết nối đến các máy chủ EC2 chúng ta đã tạo bằng **Session Manager**.