---
title : "Inspector - Bảng điều khiển"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.3.2 </b> "
---

#### Kiểm tra bảng điều khiển của Inspector

1. Điều hướng đến Inspector **Dashboard**. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/dashboard 


2. Ở đầu trang, bạn có thể thấy **Environment coverage**. Lưu ý rằng chỉ có một phần trăm các Instance được bảo vệ. Bạn có thể chọn phần trăm đó để xem chi tiết hơn về các instance nào của bạn đang được quét.

![VPC](/images/2/2.3/2.3.2/s2.png)

3. Trong bảng điều khiển, hãy kiểm tra **Risk Based Remediations** gần đầu trang. Phần khắc phục dựa trên rủi ro hiển thị năm phần mềm hàng đầu với các lỗ hổng nghiêm trọng ảnh hưởng đến nhiều tài nguyên nhất trong môi trường của bạn. Khắc phục những phần mềm này có thể giảm đáng kể số lượng rủi ro nghiêm trọng đối với môi trường của bạn. Chọn tên  phần mềm để xem chi tiết lỗ hổng liên quan và các tài nguyên bị ảnh hưởng.
![VPC](/images/2/2.3/2.3.2/s3.png)


4. Trong bảng điều khiển, bạn cũng có thể xem xét:
- Các kho lưu trữ Amazon ECR có nhiều phát hiện nghiêm trọng nhất
- Các Amazon ECR container image có nhiều phát hiện nghiêm trọng nhất
- Các Amazon EC2 Instance có nhiều phát hiện nghiêm trọng nhất
- Các Amazon Machine Image (AMI) có nhiều phát hiện nghiêm trọng nhất
- Các hàm AWS Lambda có nhiều phát hiện nghiêm trọng nhất
- Trình phát hiện mã để quét mã Lambda
![VPC](/images/2/2.3/2.3.2/s4.png)

Hãy xem qua một số phát hiện. Chọn một phát hiện để xem chi tiết hơn.
![VPC](/images/2/2.3/2.3.2/e1.png)
Thêm một số thông tin
![VPC](/images/2/2.3/2.3.2/e2.png)
Bạn có thể chọn techniques để biết thêm thông tin.
![VPC](/images/2/2.3/2.3.2/e3.png)