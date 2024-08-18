---
title : "GuardDuty - Xây dựng danh sách mối đe dọa của riêng bạn"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.4 </b> "
---

#### Triển khai danh sách mối đe dọa với địa chỉ giả


1. Truy cập Amazon S3 trong AWS Console.


2. Tìm bucket có chứa "**-threatlistbucket-**" trong tên và chọn tên đó.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s2.png)

Đây là bucket mà tài khoản AWS Workshop đã thiết lập.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s2b.png)
3. Tải lên danh sách mối đe dọa mà bạn đã tạo trước đó vào S3 bucket.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s3.png)


1. Ghi lại S3 URI của tệp bạn vừa tải lên. Để tìm URI, mở bucket, nhấp để mở tệp, và sao chép S3 URI.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s4.png)


1. Bây giờ, chúng ta sẽ thêm danh sách bạn đã tải lên S3 bucket vào danh sách mối đe dọa trong GuardDuty. Quay lại Amazon GuardDuty.


6. Trong thanh điều hướng bên trái của GuardDuty, chọn **Lists**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s6.png)



7. Chọn nút **Add a threat IP list**.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s7.png)

1. Trong ô tên List name, đặt tên cho danh sách của bạn bằng một tên dễ nhớ như "Test Threat List".


9. Dưới loại vị trí, nhập S3 URI của tệp đã tải lên mà bạn đã ghi lại trước đó.


10. Chọn **Plaintext** làm định dạng.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s10.png)


11. Chọn **I Agree** và sau đó chọn **Add list**


12. Bạn sẽ cần kích hoạt danh sách sau khi nó đã được thêm vào GuardDuty. Chọn danh sách mới thêm. Chọn **Actions**, và sau đó chọn **Activate**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s12.png)


13. Một thanh trạng thái màu xanh lá cây sẽ xuất hiện nói "Successfully updated the list...".

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s13.png)
