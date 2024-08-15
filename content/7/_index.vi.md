---
title : "Quản lý session logs"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4. </b> "
---


Với Session Manager chúng ta có thể xem được lịch sử các kết nối tới các instance thông qua **Session history**. Tuy nhiên chúng ta chưa xem được chi tiết các câu lệnh được sử dụng.

![S3](/images/4.s3/001-s3.png)

Trong phần này chúng ta sẽ tiến hành tạo S3 bucket và thực hiện cấu hình lưu trữ các session logs để xem được chi tiết các câu lệnh được sử dụng trong session.

![port-fwd](/images/arc-log.png) 

### Nội dung:

  - [Cập nhật IAM Role](./4.1-updateiamrole/)
  - [Tạo **S3 Bucket**](./4.2-creates3bucket/)
  - [Tạo S3 Gateway endpoint](./4.3-creategwes3)
  - [Cấu hình **Session logs**](./4.4-configsessionlogs/)
