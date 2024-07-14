---
title : "Tạo Endpoint ec2messages"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 3.2.2.3 </b> "
---


#### Tạo VPC Endpoint EC2MESSAGES

1. Truy cập đến [giao diện quản trị của dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.
  
2. Tại trang **Create endpoint**.
  + Tại mục **Name tag** điền **SSMMESSAGES**.
  + Tại mục **Service Category** chọn **AWS Services**.
  + Tại mục **Service Name**,
  + Tại mục **Service category** chọn:  **AWS services**
  + Tại mục **Service Name** nhập: **ec2** sau đó chọn **Service Name: com.amazonaws.ap-southeast-1.ec2messages**.

![Connect](/images/3.connect/015-connect.png)

3. Tại cột **Service Name**, click chọn **com.amazonaws.ap-southeast-1.ec2messages**.
  + Tại mục **VPC**, chọn **Lab VPC**.
  + Chọn AZ đầu tiên, sau đó chọn subnet **Lab Private Subnet**.
  
![Connect](/images/3.connect/016-connect.png)

4. Kéo chuột xuống dưới.
  + Tại mục **Security Group**, chọn Security group **SG VPC Endpoint** mà chúng ta đã tạo trước đó.
  + Tại mục **Policy**, chọn **Full access**

5. Kéo chuột xuống dưới cùng.
  + Click **Create endpoint**.

6. Chúng ta đã tạo xong VPC Interface Endpoint  **EC2MESSAGES**.

7. Đảm bảo 3 endpoint cần thiết đã được tạo như hình dưới.

![Connect](/images/3.connect/018-connect.png)