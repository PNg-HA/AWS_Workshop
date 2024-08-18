---
title : "Detective - Điều tra"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4.4 </b> "
---

#### Xác định một tài nguyên để điều tra

1. Mở **IAM console** và điều hướng đến trang **Roles**. https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles

 
2. Tìm kiếm role bắt đầu bằng "**cfn-GuardDutyLabsInfrastructure-GeneralInstanceRole-**".



3. Chọn tên role.
![VPC](/images/2/2.4/2.4.4/s3.png)


4. Sao chép ARN từ đầu trang. Bạn sẽ cần ARN này trong bước sau.
![VPC](/images/2/2.4/2.4.4/s4.png)
#### Thực hiện một cuộc điều tra bằng Detective

1. Mở trang **Investigations** từ **Detective**. https://us-east-1.console.aws.amazon.com/detective/home?region=us-east-1#investigations 


2. Trong trang Investigations, chọn **Run investigation** ở góc trên bên phải.
![VPC](/images/2/2.4/2.4.4/s6.png)

1. Trong phần Select resource, bạn có ba cách để thực hiện cuộc điều tra. Bạn có thể chọn chạy cuộc điều tra cho một tài nguyên được Detective khuyến nghị. Bạn có thể chạy cuộc điều tra cho một tài nguyên cụ thể. Bạn cũng có thể điều tra một tài nguyên từ trang Detective Search. Chọn **Specify an AWS role or user with an ARN**.



8. Dưới **Select resource type** chọn **AWS role**.


9. Dưới **Resource ARN**, nhập ARN của tài nguyên mà bạn đã xác định ở trên.



10. Chọn **Run investigation**.
![VPC](/images/2/2.4/2.4.4/s11.png)


11. Chờ cho cuộc điều tra hoàn tất. Nó sẽ hiển thị trạng thái "Successful" trong vòng 2-3 phút.
![VPC](/images/2/2.4/2.4.4/s12a.png)



12. Chọn ID của cuộc điều tra để xem kết quả. Tùy thuộc vào thời gian mà tài khoản này đã được cung cấp, bạn có thể thấy "We did not observe uncommon behavior...". Tuy nhiên, nếu có điều gì đó đã được phát hiện, bạn sẽ thấy báo cáo chứa phần Indicators of Compromise sbao gồm các chi tiết liên quan đến một hoặc nhiều indicators of compromise. Tóm tắt điều tra làm nổi bật các indicators bất thường cần được chú ý, cho phạm vi thời gian được chọn. Bằng cách sử dụng bản tóm tắt, bạn có thể nhanh chóng xác định nguyên nhân cốt lõi của các vấn đề bảo mật tiềm ẩn, xác định các mẫu và hiểu rõ các tài nguyên bị ảnh hưởng bởi các sự kiện bảo mật.

![VPC](/images/2/2.4/2.4.4/s12b.png)
![VPC](/images/2/2.4/2.4.4/s12c.png)

Nếu bạn muốn tìm thêm chi tiết về loại **IP address**, bạn sẽ thấy các TTP khác nhau:
![VPC](/images/2/2.4/2.4.4/s12d.png)

Loại TTP:
![VPC](/images/2/2.4/2.4.4/s12e.png)