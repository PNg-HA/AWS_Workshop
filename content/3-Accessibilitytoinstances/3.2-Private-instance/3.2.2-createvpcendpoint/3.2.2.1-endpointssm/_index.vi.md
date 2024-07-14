---
title : "Tạo Endpoint ssm"
date :  "`r Sys.Date()`" 
weight : 1
chapter : false
pre : " <b> 3.2.2.1 </b> "
---


#### Tạo VPC Endpoint SSM

1. Truy cập đến [giao diện quản trị của dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.
  
![Connect](/images/3.connect/005-connect.png)

2. Tại trang **Create endpoint**.
  + Tại mục **Name tag** điền **SSM**.
  + Tại mục **Service Category** chọn **AWS Services**.
  + Tại mục **Service Name**,
  + Tại mục **Service category** chọn:  **AWS services**
  + Tại mục **Service Name** nhập: **SSM** sau đó chọn **Service Name: com.amazonaws.ap-southeast-1.ssm**.

![Connect](/images/3.connect/006-connect.png)

3. Tại cột **Service Name**, click chọn **com.amazonaws.ap-southeast-1.ssm**.
  + Tại mục **VPC**, chọn **Lab VPC**.
  + Chọn AZ đầu tiên, sau đó chọn subnet **Lab Private Subnet**.
  
![Connect](/images/3.connect/007-connect.png)

4. Kéo chuột xuống dưới.
  + Tại mục **Security Group**, chọn Security group **SG VPC Endpoint** mà chúng ta đã tạo trước đó.
  + Tại mục **Policy**, chọn **Full access**

![Connect](/images/3.connect/008-connect.png)

5. Kéo chuột xuống dưới cùng.
  + Click **Create endpoint**.

6. Chúng ta đã tạo xong VPC Interface Endpoint cho **SSM**.


![Connect](/images/3.connect/011-connect.png)
