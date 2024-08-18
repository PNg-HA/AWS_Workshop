---
title : "GuardDuty - Ẩn phát hiện"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.2.5 </b> "
---

#### Cấu hình quy tắc để ẩn các phát hiện không mong muốn cho công cụ đánh giá lỗ hổng của bạn

1. Từ bảng điều khiển GuardDuty, mở trang **Findings**.


2. Chọn nút **Suppress Findings**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s2.png)



3. Quy tắc ẩn nên bao gồm hai tiêu chí lọc. Tiêu chí đầu tiên nên sử dụng thuộc tính Loại phát hiện với giá trị là Recon:EC2/Portscan. Trong trường **Add filter criteria**, chọn **Finding Type** và nhập **"Recon:EC2/Portscan"**. Chọn **Apply**.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s3.png)

4. Lưu ý rằng sau khi thêm bộ lọc đầu tiên, bản xem trước ở dưới cùng của trang sẽ hiển thị các phát hiện sẽ bị ẩn bởi quy tắc này. Để đảm bảo rằng bạn không vô tình ẩn một phát hiện mà bạn muốn nhận thông báo, hãy thêm tiêu chí lọc thứ hai để thu hẹp quy tắc.


5. Tiêu chí lọc thứ hai nên khớp với instance hoặc các instance lưu trữ các công cụ đánh giá lỗ hổng này. Bạn có thể sử dụng thuộc tính Instance image ID  hoặc thuộc tính Tag value attribute tùy thuộc vào tiêu chí nào có thể nhận diện các instance lưu trữ các công cụ này. Chọn **Add filter criteria** một lần nữa, và chọn **Instance Tag Value** nhập "**Scanner**". Chọn **Apply**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s5.png)


6. Lưu ý rằng chỉ một phát hiện sẽ bị ẩn bởi quy tắc này.


7. Nhập "**VulnerabilityAssessmentTool**"  vào trường **Name**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s7.png)


1. Nhập "**Suppress findings from Vulnerability Assessment Tool.**" vào trường **Description**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s8.png)


9. Chọn **Save**.