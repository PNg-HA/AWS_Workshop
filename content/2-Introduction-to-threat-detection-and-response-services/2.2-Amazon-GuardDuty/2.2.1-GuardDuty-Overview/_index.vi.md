---
title : "GuardDuty - Tổng quan"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.2.1 </b> "
---


#### GuardDuty - Tổng quan
Amazon GuardDuty là một dịch vụ phát hiện mối đe dọa liên tục giám sát hoạt động độc hại và hành vi trái phép trong toàn bộ môi trường AWS của bạn. GuardDuty kết hợp học máy (ML), phát hiện bất thường, và phát hiện tệp độc hại, sử dụng cả các nguồn từ AWS và các bên thứ ba hàng đầu trong ngành để giúp bảo vệ tài khoản AWS, khối lượng công việc và dữ liệu của bạn. GuardDuty có khả năng phân tích hàng chục tỷ sự kiện từ nhiều nguồn dữ liệu AWS, bao gồm nhật ký AWS CloudTrail, Amazon Virtual Private Cloud (Amazon VPC) Flow Logs, và nhật ký truy vấn DNS. GuardDuty cũng giám sát các sự kiện dữ liệu của Amazon Simple Storage Service (Amazon S3), sự kiện đăng nhập Amazon Aurora, và hoạt động runtime cho Amazon Elastic Kubernetes Service (Amazon EKS), Amazon Elastic Compute Cloud (Amazon EC2), và Amazon Elastic Container Service (Amazon ECS) — bao gồm cả khối lượng công việc container không máy chủ trên AWS Fargate.

GuardDuty cung cấp khả năng phát hiện mối đe dọa chính xác cho các tài khoản bị xâm nhập, điều này có thể khó phát hiện nhanh chóng nếu bạn không giám sát liên tục các yếu tố gần như theo thời gian thực. GuardDuty có thể phát hiện dấu hiệu của việc xâm nhập tài khoản, chẳng hạn như truy cập tài nguyên AWS từ một vị trí địa lý bất thường vào thời điểm không điển hình trong ngày. Đối với các tài khoản AWS có truy cập chương trình, GuardDuty kiểm tra các cuộc gọi API bất thường, chẳng hạn như các nỗ lực che giấu hoạt động tài khoản bằng cách vô hiệu hóa nhật ký CloudTrail hoặc chụp nhanh cơ sở dữ liệu từ một địa chỉ IP độc hại.

GuardDuty liên tục giám sát và phân tích dữ liệu sự kiện tài khoản AWS và khối lượng công việc của bạn được tìm thấy trong CloudTrail, VPC Flow Logs, và DNS logs. Không có phần mềm bảo mật bổ sung hoặc cơ sở hạ tầng nào cần triển khai và bảo trì cho các bảo vệ cơ bản trong GuardDuty. Bằng cách kết hợp các tài khoản AWS của bạn lại với nhau, bạn có thể tổng hợp phát hiện mối đe dọa thay vì làm việc trên cơ sở từng tài khoản. Ngoài ra, bạn không cần phải thu thập, phân tích và liên kết một lượng lớn dữ liệu AWS từ nhiều tài khoản. Hãy tập trung vào cách phản ứng nhanh chóng, cách giữ an toàn cho tổ chức của bạn và tiếp tục mở rộng quy mô và đổi mới trên AWS.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.1-GuardDuty-Overview/s1.png)