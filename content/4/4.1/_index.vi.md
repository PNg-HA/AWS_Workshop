---
title : "Cập nhật IAM Role"
date :  "`r Sys.Date()`" 
weight : 1 
chapter : false
pre : " <b> 4.1 </b> "
---

Để các EC2 instance của chúng ta có thể gửi session log tới S3 bucket , chúng ta sẽ cần cập nhật IAM Role đã gán vào EC2 instance bằng cách thêm vào policy cho phép quyền truy cập vào S3.

#### Cập nhật IAM Role

1. Truy cập vào [giao diện quản trị dịch vụ IAM](https://console.aws.amazon.com/iamv2/home?#/home)
  + Click **Roles**.
  + Tại ô tìm kiếm , điền **SSM**.
  + Click vào role **SSM-Role**.

![S3](/images/4.s3/002-s3.png)

2. Click **Attach policies**.
 
![S3](/images/4.s3/003-s3.png)

3. Tại ô Search điền **S3**.
  + Click chọn policy **AmazonS3FullAccess**.
  + Click **Attach policy**.
 
![S3](/images/4.s3/004-s3.png)
 
{{%notice tip%}}
Trong thực tế chúng ta sẽ cấp quyền chặt chẽ hơn tới S3 bucket chỉ định. Trong khuôn khổ bài lab này chúng ta sử dụng policy **AmazonS3FullAccess** cho tiện dụng.
{{%/notice%}}

Tiếp theo chúng ta sẽ tiến hành tạo S3 bucket để lưu trữ session logs.