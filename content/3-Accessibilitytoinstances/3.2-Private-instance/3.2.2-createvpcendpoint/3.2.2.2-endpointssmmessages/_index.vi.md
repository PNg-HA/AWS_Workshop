---
title : "Tạo Endpoint ssmmessages"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2.2.2 </b> "
---


#### Tạo VPC Endpoint SSMMESSAGES

1. Truy cập đến [giao diện quản trị của dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.
  
2. Tại trang **Create endpoint**.
  + Tại mục **Name tag** điền **SSMMESSAGES**.
  + Tại mục **Service Category** chọn **AWS Services**.
  + Tại mục **Service Name**,
  + Tại mục **Service category** chọn:  **AWS services**
  + Tại mục **Service Name** nhập: **ssmmessages** sau đó chọn **Service Name: com.amazonaws.ap-southeast-1.ssmmessages**.

![Connect](/images/3.connect/012-connect.png)

3. Tại cột **Service Name**, click chọn **com.amazonaws.ap-southeast-1.ssmmessages**.
  + Tại mục **VPC**, chọn **Lab VPC**.
  + Chọn AZ đầu tiên, sau đó chọn subnet **Lab Private Subnet**.
  
![Connect](/images/3.connect/013-connect.png)

4. Kéo chuột xuống dưới.
  + Tại mục **Security Group**, chọn Security group **SG VPC Endpoint** mà chúng ta đã tạo trước đó.
  + Tại mục **Policy**, chọn **Full access**

5. Kéo chuột xuống dưới cùng.
  + Click **Create endpoint**.

6. Chúng ta đã tạo xong VPC Interface Endpoint  **SSMMESSAGES**.
