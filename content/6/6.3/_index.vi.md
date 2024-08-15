---
title : "Tạo S3 Gateway endpoint"
date :  "`r Sys.Date()`" 
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---


1. Truy cập vào [giao diện quản trị dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.

2. Tại trang **Create endpoint**.
  + Tại mục **Name tag** điền **S3GW**.
  + Tại mục **Service Category** click chọn **AWS services**.
  + Tại ô tìm kiếm điền **S3**, sau đó chọn **com.amazonaws.[region].s3**

![S3](/images/4.s3/008-s3.png)

3. Tại mục **Services** chọn **com.amazonaws.[region].s3** có Type là **Gateway**.
  + Tại mục **VPC** , chọn **Lab VPC**.
  + Tại mục **Route tables**, chọn cả 2 route table.
  
![S3](/images/4.s3/009-s3.png)

4. Kéo chuột xuống dưới cùng, click **Create endpoint**.

Bước tiếp theo chúng ta sẽ tiến hành cấu hình Session Manager để có thể lưu trữ các session logs tới S3 bucket chúng ta đã tạo.