---
title : "Đối phó khi một hàm Lambda gọi đến IP độc hại"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 6.4 </b> "
---

{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Bạn được giao nhiệm vụ điều tra và khắc phục một phát hiện của GuardDuty. Loại phát hiện của GuardDuty là "UnauthorizedAccess:Lambda/MaliciousIPCaller.Custom".
{{%/notice%}}

Lambda Protection giúp bạn xác định các mối đe dọa bảo mật tiềm ẩn khi một hàm AWS Lambda được gọi trong môi trường AWS của bạn. Khi bạn kích hoạt Lambda Protection,  GuardDuty bắt đầu giám sát các log hoạt động mạng của Lambda, bắt đầu với VPC Flow Logs từ tất cả các hàm Lambda cho tài khoản của bạn, bao gồm cả những log không sử dụng mạng VPC, và được tạo ra khi hàm Lambda được gọi. Nếu GuardDuty xác định được lưu lượng mạng đáng ngờ cho thấy có thể có sự xuất hiện của một đoạn mã độc trong hàm Lambda của bạn, GuardDuty sẽ tạo ra một phát hiện.

GuardDuty giám sát các log hoạt động mạng được tạo ra bởi việc gọi các hàm Lambda. Giám sát Hoạt động Mạng của Lambda bao gồm Amazon VPC Flow Logs từ tất cả các hàm Lambda trong tài khoản của bạn, bao gồm cả những log không sử dụng mạng VPC, và có thể thay đổi, bao gồm mở rộng sang các hoạt động mạng khác như dữ liệu truy vấn DNS được tạo ra bởi việc gọi các hàm Lambda. Việc mở rộng sang các hình thức giám sát hoạt động mạng khác sẽ làm tăng khối lượng dữ liệu mà GuardDuty sẽ xử lý cho Lambda Protection. Điều này sẽ ảnh hưởng trực tiếp đến chi phí sử dụng Lambda Protection. Bất cứ khi nào GuardDuty bắt đầu giám sát một log hoạt động mạng bổ sung, nó sẽ cung cấp thông báo cho các tài khoản đã bật Lambda Protection ít nhất 30 ngày trước khi phát hành.

#### Tạo Security Group

1. Truy cập trang Findings trong GuardDuty. https://console.aws.amazon.com/guardduty/home?#/findings?macros=current 


2. Thêm tiêu chí lọc và chọn Resource sau đó chọn Lambda.
![VPC](/images/6/6.4/s2.png)

3. Lưu ý rằng có một phát hiện về một hàm Lambda đang gọi đến một IP độc hại. Phát hiện này được tạo ra bởi một hàm Lambda được lập lịch chạy mỗi 10 phút. Hàm này gọi đến một địa chỉ IP nằm trong danh sách IP độc hại đang hoạt động trong GuardDuty có tên là "S3BadIpAddressList".

![VPC](/images/6/6.4/s3.png)

4. CliNhấp vào phát hiện để mở và xem xét chi tiết. Bạn có thể tìm thấy tên của hàm Lambda và địa chỉ IP mà nó đang gọi từ danh sách IP độc hại không? 

Hàm Lambda:
![VPC](/images/6/6.4/s4.png)
IP:
![VPC](/images/6/6.4/s4b.png)

5. Nếu muốn xem lịch sử cấu hình của hàm, bạn có thể sử dụng AWS Config. AWS Config liên tục đánh giá, kiểm tra và đánh giá cấu hình cũng như mối quan hệ giữa các tài nguyên của bạn trên AWS, tại chỗ và trên các Cloud khác. Mở AWS Config và điều hướng đến Timeline cho hàm Lambda, hoặc theo liên kết này: https://console.aws.amazon.com/config/home?#/resources/timeline?resourceId=traffic-simulation&resourceName=traffic-simulation
![VPC](/images/6/6.4/s5.png)

6. Nhấp vào Load more ở cuối trang cho đến khi hiển thị toàn bộ lịch sử. Mở rộng sự kiện CloudTrail cũ nhất, sau đó dưới mục View event, nhấp vào CloudTrail. AWS CloudTrail giám sát và ghi lại hoạt động tài khoản trên cơ sở hạ tầng AWS của bạn, cung cấp cho bạn quyền kiểm soát lưu trữ, phân tích và hành động khắc phục.

![VPC](/images/6/6.4/s6.png)

7. Nhấp vào tên của sự kiện trong CloudTrail. Nó sẽ bắt đầu bằng "CreateFunction...". Điều này sẽ mở bản ghi sự kiện về việc tạo hàm Lambda này. Trong số các thông tin khác, bản ghi sự kiện này chi tiết rằng hàm được tạo bằng CloudFormation, từ một hàm được lưu trữ trong Amazon S3, bởi người dùng có tên WSSystemRole. 

![VPC](/images/6/6.4/s7.png)
8. Bạn có thể tùy chọn xóa hàm Lambda từ bảng điều khiển Lambda. Nếu không, hãy tiếp tục sang module tiếp theo.