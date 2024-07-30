---
title : "Tạo S3 Bucket"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 4.2 </b> "
---


Trong bước này, chúng ta sẽ tạo 1 S3 bucket để lưu trữ các session logs được gửi từ các EC2 instance.

#### Tạo **S3 Bucket**

1. Truy cập [giao diện quản trị dịch vụ S3](https://s3.console.aws.amazon.com/s3/home)
  + Click **Create bucket**.

![S3](/images/4.s3/005-s3.png)

2. Tại trang **Create bucket**.
  + Tại mục **Bucket name** điền tên bucket **lab-yourname-bucket-0001**
  + Tại mục **Region** chọn **Region** bạn đang làm lab hiện tại. 

![S3](/images/4.s3/006-s3.png)

 {{%notice tip%}}
Tên S3 bucket phải đảm bảo không trùng với toàn bộ S3 bucket khác trong hệ thống. Bạn có thể thay thế tên mình và điền số ngẫu nhiên khi tạo tên S3 bucket.
{{%/notice%}}

3. Kéo chuột xuống phía dưới và click **Create bucket**.

![S3](/images/4.s3/007-s3.png)

 {{%notice tip%}}
Khi tạo S3 bucket chúng ta đã thực hiện **Block all public access** nên các EC2 instance của chúng ta sẽ không thể kết nối tới S3 thông qua mạng internet.
Trong bước tiếp theo chúng ta sẽ cấu hình tính năng S3 Gateway Endpoint để cho phép các EC2 instance có thể kết nối tới S3 bucket thông qua mạng nội bộ của VPC.
{{%/notice%}}
