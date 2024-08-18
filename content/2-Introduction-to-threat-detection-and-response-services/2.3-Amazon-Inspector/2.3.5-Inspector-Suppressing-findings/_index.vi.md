---
title : "Inspector - Ẩn phát hiện"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.3.5 </b> "
---
Sử dụng quy tắc ẩn để ẩn các phát hiện khớp với tiêu chí. Ví dụ: bạn có thể tạo quy tắc ẩn tất cả các phát hiện có điểm dễ bị tổn thương thấp, do đó bạn chỉ có thể tập trung vào các phát hiện quan trọng nhất.

Quy tắc ẩn chỉ được sử dụng để lọc danh sách các phát hiện của bạn và không có bất kỳ tác động nào đến các phát hiện hoặc ngăn Amazon Inspector tạo ra các phát hiện.

Nếu Amazon Inspector tạo ra các phát hiện khớp với quy tắc ẩn, các phát hiện sẽ được đặt thành Suppressed. Các phát hiện khớp với quy tắc ẩn sẽ không xuất hiện trong danh sách của bạn theo mặc định.
#### Cấu hình một rule để ẩn các phát hiện có mức độ nguy hiểm thấp trong môi trường dev



1. Chọn **Suppression rules** từ thanh điều hướng bên trái trong Inspector.


2. Chọn **Create Rule**.
![VPC](/images/2/2.3/2.3.5/s2.png)


3. Nhập "**Low Severity - Dev Environment**" vào trường **Name**.



4. Nhập "**Suppress all findings with a low severity score for resources tagged with Development**" vào trường **Description**.
![VPC](/images/2/2.3/2.3.5/s4.png)

5. Trong phần **Suppression rule filters** chọn **Severity** và đánh dấu vào ô **Low**. Chọn **Apply**.
![VPC](/images/2/2.3/2.3.5/s5.png)


6. Sau đó, nhấp để thêm một bộ lọc khác và chọn **Resource tag** dưới EC2. Nhập "**Environment**" cho key và "**Development**" cho **value**. Chọn **Apply**.


7. Chọn **Save**.
Kết quả:
![VPC](/images/2/2.3/2.3.5/s5b.png)

8. Bạn có thể xem các phát hiện đã bị ẩn trong [trang **All Findings**](https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/findings/all) bằng cách chuyển từ **Active** sang **Suppressed** trong menu thả xuống.