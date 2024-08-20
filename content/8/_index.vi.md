+++
title = "Dọn dẹp tài nguyên"
date = 2024
weight = 8
chapter = false
pre = "<b>8. </b>"
+++

Chúng ta sẽ thực hiện các bước sau để xóa các tài nguyên đã tạo trong bài thực hành này.

#### Xóa giải pháp Automated Security Response

1. Xóa aws-sharr-member.template khỏi từng tài khoản thành viên.

2. Xóa aws-sharr-admin.template khỏi tài khoản quản trị viên.
3. Xóa các IAM role có tên SO0111-*
4. Xóa ghi log trên CloudWatch.

#### Vô hiệu hóa AWS Security Hub
1. Truy cập [trang cài đặt chung của Security Hub](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/settings/general).
2. Nhấp vào **Disable AWS Security Hub**. 
![VPC](/images/8/sh1.png)
3. Sẽ có một cửa sổ bật lên, nhấp vào **Disable AWS Security Hub**. 
![VPC](/images/8/sh2.png)



#### Xóa các dịch vụ liên quan
1. Vô hiệu hóa GuardDuty.
2. Vô hiệu hóa Inspector.
3. Vô hiệu hóa Detective.