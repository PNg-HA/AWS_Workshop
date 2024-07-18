---
title : "Tạo Public subnet"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 2.1.2 </b> "
---

#### Tạo Public subnet

1. Click **Subnets**.
  + Click **Create subnet**.

![VPC](/images/2.prerequisite/003-createsubnet.png)

2. Tại trang **Create subnet**.
  + Tại mục **VPC ID** click chọn **Lab VPC**.
  + Tại mục **Subnet name** điền **Lab Public Subnet**.
  + Tại mục **Availability Zone** chọn Availability zone đầu tiên.
  + Tại mục **IPv4 CIRD block** điền **10.10.1.0/24**.

![VPC](/images/2.prerequisite/004-createsubnet.png)

3. Kéo xuống cuối trang , click **Create subnet**.

4. Click chọn **Lab Public Subnet**.
  + Click **Actions**.
  + Click **Edit subnet settings**.

![VPC](/images/2.prerequisite/005-createsubnet.png)

5. Click chọn **Enable auto-assign public IPv4 address**.
  + Click **Save**.

![VPC](/images/2.prerequisite/006-createsubnet.png)

6. Click **Internet Gateways**.
  + Click **Create internet gateway**.
  
![VPC](/images/2.prerequisite/007-createigw.png)

7. Tại trang **Create internet gateway**.
  + Tại mục **Name tag** điền **Lab IGW**.
  + Click **Create internet gateway**.
  
![VPC](/images/2.prerequisite/008-createigw.png)

8. Sau khi tạo thành công, click **Actions**.
  + Click **Attach to VPC**.
 
![VPC](/images/2.prerequisite/009-createigw.png)

9. Tại trang **Attach to VPC**.
  + Tại mục **Available VPCs** chọn **Lab VPC**.
  + Click **Attach internet gateway**.
  + Kiểm tra quá trình attach thành công như hình dưới.

![VPC](/images/2.prerequisite/010-createigw.png)

10. Tiếp theo chúng ta sẽ tạo một custom route table để gán vào **Lab Public Subnet**.
  + Click **Route Tables**.
  + Click **Create route table**.

![VPC](/images/2.prerequisite/011-creatertb.png)

11. Tại trang **Create route table**.
  + Tại mục **Name**, điền **Lab Publicrtb**.
  + Tại mục **VPC**, chọn **Lab VPC**.
  + Click **Create route table**.

12. Sau khi tạo route table thành công.
  + Click **Edit routes**.
  
![VPC](/images/2.prerequisite/012-creatertb.png)

13. Tại trang **Edit routes**.
  + Click **Add route**.
  + Tại mục **Destination** điền 0.0.0.0/0
  + Tại mục **Target** chọn **Internet Gateway** sau đó chọn **Lab IGW**.
  + Click **Save changes**.

![VPC](/images/2.prerequisite/013-creatertb.png)

14. Click tab **Subnet associations**.
  + Click **Edit subnet associations** để tiến hành associate custom routable chúng ta vừa tạo vào **Lab Public Subnet**.


![VPC](/images/2.prerequisite/014-creatertb.png)

15. Tại trang **Edit subnet associations**. 
  + Click chọn **Lab Public Subnet**.
  + Click **Save associations**.

![VPC](/images/2.prerequisite/015-creatertb.png)

16. Kiểm tra thông tin route table đã được associate với **Lab Public Subnet** và thông tin route đi internet đã được trỏ đến Internet Gateway như hình dưới.


![VPC](/images/2.prerequisite/016-creatertb.png)
