---
title : "Theo dõi session logs"
date :  "`r Sys.Date()`" 
weight : 4 
chapter : false
pre : " <b> 4.4 </b> "
---

#### Theo dõi session logs

1. Truy cập [giao diện quản trị dịch vụ System Manager - Session Manager](https://console.aws.amazon.com/systems-manager/session-manager)
  + Click tab **Preferences**.
  + Click **Edit**.
  
![S3](/images/4.s3/010-s3.png)

2. Kéo chuột xuống phía dưới, tại mục **S3 logging**, click chọn **Enable**.
  + Bỏ chọn **Allow only encrypted S3 buckets**.
  + Click chọn **Choose a bucket name from the list**.
  + Chọn S3 bucket bạn đã tạo.
  
![S3](/images/4.s3/011-s3.png)

3. Kéo chuột xuống phía dưới, click **Save** để lưu cấu hình.

4. Truy cập [giao diện quản trị dịch vụ System Manager - Session Manager](https://console.aws.amazon.com/systems-manager/session-manager)
  + Click **Start session**.
  + Click chọn  **Private Windows Instance**.
  + Click **Start session**.

5. Gõ lệnh **ipconfig**.
  + Gõ lệnh **hostname**.
  + Click **Terminate** để thoát session, click **Terminate** 1 lần nữa để xác nhận.

![S3](/images/4.s3/012-s3.png)


#### Kiểm tra **Session logs** trong **S3**

1. Truy cập vào [giao diện quản trị dịch vụ S3](https://s3.console.aws.amazon.com/s3/home)
  + Click vào tên S3 bucket chúng ta đã tạo cho bài lab.

2. Click vào tên file sessions log

![S3](/images/4.s3/013-s3.png)

3. Tại trang chi tiết objects , click **Open**.

![S3](/images/4.s3/014-s3.png)

4. File logs sẽ được mở ở 1 tab mới trên trình duyệt.Bạn có thể xem các câu lệnh đã được lưu trữ lại trong  session logs.

![S3](/images/4.s3/015-s3.png)

