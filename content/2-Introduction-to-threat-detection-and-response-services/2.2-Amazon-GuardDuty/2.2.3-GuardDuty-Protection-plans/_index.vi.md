---
title : "Tạo Private subnet"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

#### Tạo Private subnet

1. Click **Subnets**.
  + Click **Create subnet**.

![VPC](/images/2.prerequisite/017-createsubnet.png)

2. Tại trang **Create subnet**.
  + Tại mục **VPC ID** click chọn **Lab VPC**.
  + Tại mục **Subnet name** điền **Lab Private Subnet**.
  + Tại mục **Availability Zone** chọn Availability zone đầu tiên.
  + Tại mục **IPv4 CIRD block** điền **10.10.2.0/24**.

![VPC](/images/2.prerequisite/018-createsubnet.png)

3. Kéo xuống cuối trang , click **Create subnet**.

Bước tiếp theo chúng ta sẽ tạo các security group cần thiết cho bài lab.