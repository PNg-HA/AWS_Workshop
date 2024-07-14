---
title : "Các bước chuẩn bị"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 2. </b> "
---

{{% notice info %}}
Bạn cần tạo sẵn 1 Linux instance thuộc public subnet và 1 Window instance thuộc private subnet để thực hiện bài thực hành này.
{{% /notice %}}

Để tìm hiểu cách tạo các EC2 instance và VPC với public/private subnet các bạn có thể tham khảo bài lab :
  - [Giới thiệu về Amazon EC2](https://000004.awsstudygroup.com/vi/)
  - [Làm việc với Amazon VPC](https://000003.awsstudygroup.com/vi/)

Để sử dụng System Manager để quản lý window instance nói riêng và các instance nói chung của chúng ta trên AWS, ta cần phải cung cấp quyền cho các instance của chúng ta có thể làm việc với System Manager.Trong phần chuẩn bị này, chúng ta cũng sẽ tiến hành tạo IAM Role để cấp quyền cho các instance có thể làm việc với System Manager.

### Nội dung
  - [Chuẩn bị VPC và EC2 Instance](2.1-createec2/)
  - [Tạo IAM Role](2.2-createiamrole/)

  
