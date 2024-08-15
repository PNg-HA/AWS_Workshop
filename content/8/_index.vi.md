+++
title = "Dọn dẹp tài nguyên  "
date = 2021
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

Chúng ta sẽ tiến hành các bước sau để xóa các tài nguyên chúng ta đã tạo trong bài thực hành này.

#### Xóa EC2 instance

1. Truy cập [giao diện quản trị dịch vụ EC2](https://console.aws.amazon.com/ec2/v2/home)
  + Click **Instances**.
  + Click chọn cả 2 instance **Public Linux Instance** và **Private Windows Instance**. 
  + Click **Instance state**.
  + Click **Terminate instance**, sau đó click **Terminate** để xác nhận.

2. Truy cập [giao diện quản trị dịch vụ IAM](https://console.aws.amazon.com/iamv2/home#/home)
  + Click **Roles**.
  + Tại ô tìm kiếm , điền **SSM**.
  + Click chọn **SSM-Role**.
  + Click **Delete**, sau đó điền tên role **SSM-Role** và click **Delete** để xóa role.
  
![Clean](/images/6.clean/001-clean.png)

3. Click **Users**.
  + Click chọn user **Portfwd**.
  + Click **Delete**, sau đó điền tên user **Portfwd** và click **Delete** để xóa user.

#### Xóa S3 bucket

1. Truy cập [giao diện quản trị dịch vụ System Manager - Session Manager](https://console.aws.amazon.com/systems-manager/session-manager).
  + Click tab **Preferences**.
  + Click **Edit**.
  + Kéo chuột xuống dưới.
  + Tại mục **S3 logging**.
  + Bỏ chọn **Enable** để tắt tính năng logging.
  + Kéo chuột xuống dưới.
  + Click **Save**.

2. Truy cập [giao diện quản trị dịch vụ S3](https://s3.console.aws.amazon.com/s3/home)
  + Click chọn S3 bucket chúng ta đã tạo cho bài thực hành. ( Ví dụ : lab-fcj-bucket-0001 )
  + Click **Empty**.
  + Điền **permanently delete**, sau đó click **Empty** để tiến hành xóa object trong bucket.
  + Click **Exit**.

3. Sau khi xóa hết object trong bucket, click **Delete**

![Clean](/images/6.clean/002-clean.png)

4. Điền tên S3 bucket, sau đó click **Delete bucket** để tiến hành xóa S3 bucket.

![Clean](/images/6.clean/003-clean.png)

#### Xóa các VPC Endpoint

1. Truy cập vào [giao diện quản trị dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Chọn 4 endpoints chúng ta đã tạo cho bài thực hành bao gồm **SSM**, **SSMMESSAGES**, **EC2MESSAGES**, **S3GW**.
  + Click **Actions**.
  + Click **Delete VPC endpoints**.

![Clean](/images/6.clean/004-clean.png)

2. Tại ô confirm , điền **delete**.
  + Click **Delete** để tiến hành xóa các endpoints.

3. Click biểu tượng refresh, kiểm tra tất cả các endpoints đã bị xóa trước khi làm bước tiếp theo.

![Clean](/images/6.clean/005-clean.png)

#### Xóa VPC

1. Truy cập vào [giao diện quản trị dịch vụ VPC](https://console.aws.amazon.com/vpc/home)
  + Click **Your VPCs**.
  + Click chọn **Lab VPC**.
  + Click **Actions**.
  + Click **Delete VPC**.

2. Tại ô confirm, điền **delete** để xác nhận, click **Delete** để thực hiện xóa **Lab VPC** và các tài nguyên liên quan.

![Clean](/images/6.clean/006-clean.png)