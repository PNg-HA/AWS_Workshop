---
title : "Ứng phó với S3 Bucket bị xâm phạm"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 6.2 </b> "
---
{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: 
Bạn được giao nhiệm vụ điều tra và khắc phục một phát hiện từ GuardDuty. Loại phát hiện GuardDuty là "Stealth:S3/ServerAccessLoggingDisabled".
{{%/notice%}}


1. Điều hướng đến [bảng điều khiển GuardDuty](https://us-east-1.console.aws.amazon.com/guardduty/home?region=us-east-1#/findings) và mở trang Findings. Bạn sẽ thấy các phát hiện với loại bắt đầu bằng Stealth:S3 và Policy:S3. Nếu bạn không thấy những phát hiện này, hãy thử làm mới trang.
![VPC](/images/6/6.2/s1.png)
2. Nhấp vào phát hiện Stealth:S3/ServerAccessLoggingDisabled để xem chi tiết đầy đủ. Bạn có thể thấy chi tiết phát hiện bao gồm thông tin về những gì đã xảy ra, những tài nguyên AWS nào có liên quan đến hoạt động đáng ngờ, thời điểm diễn ra hoạt động này và các thông tin bổ sung khác. Dưới phần Resource Affected, tìm xem S3 bucket nào liên quan đến phát hiện này, IAM User (User Name) nào đã thực hiện thay đổi và Access key ID đã được sử dụng.
![VPC](/images/6/6.2/s2.png)
Phát hiện này thông báo cho bạn rằng tính năng ghi log truy cập máy chủ của S3 đã bị tắt cho bucket. Nếu bị tắt, không có log nào được tạo cho bất kỳ hành động nào được thực hiện trên S3 bucket hoặc trên các object trong bucket, trừ khi tính năng ghi log cấp object S3 được bật. Mặc dù đây là phát hiện có mức độ nghiêm trọng thấp, nhưng điều quan trọng là phải điều tra lý do tại sao ai đó lại tắt tính năng ghi log. Việc tắt tính năng ghi log có thể được sử dụng để che giấu những thay đổi không nên thực hiện. Để tìm hiểu thêm, hãy xem [S3 Server Access Logging](https://docs.aws.amazon.com/AmazonS3/latest/dev/ServerLogs.html).



3. Điều tra một phát hiện S3 Policy khác. Nhấp vào phát hiện Policy:S3/BucketBlockPublicAccessDisabled. Phát hiện này được tạo ra khi cài đặt chặn quyền truy cập công cộng bị tắt nếu chúng đã được bật trước đó. Đây có thể là một thay đổi có chủ ý hoặc có thể là một thay đổi vô tình hoặc cố ý đối với cài đặt bucket. Trong tình huống như vậy, tốt nhất bạn nên xác nhận với chủ sở hữu bucket xem đây có phải là cố ý hay không. Với bối cảnh về việc bucket được sử dụng để làm gì, điều này có vẻ đáng ngờ.
![VPC](/images/6/6.2/s3.png)


4. Để xác định các hành động mà IAM User đã thực hiện, hãy nhấp vào từng phát hiện. Tìm các lệnh API đã được thực hiện bằng cách nhìn dưới phần Action và sau đó là API. Đây là các sự kiện quản lý hay sự kiện data plane? GuardDuty có thể phân tích khối lượng dữ liệu lớn và xác định các mối đe dọa tiềm ẩn trong môi trường của bạn. Tuy nhiên, điều quan trọng vẫn là phải liên kết các dữ liệu khác để hiểu rõ hơn phạm vi tiềm ẩn của mối đe dọa. Trong trường hợp này, bạn sẽ sử dụng các chi tiết trong phát hiện này để xác định hoạt động người dùng trong quá khứ trong AWS CloudTrail. Bạn có thể điều tra thêm và phân tích nguyên nhân cốt lõi của phát hiện bảo mật này với [Amazon Detective](https://aws.amazon.com/detective/).

Những phát hiện IAM này được tạo ra bởi "malicious EC2" instance thực hiện các lệnh API. GuardDuty tạo ra các phát hiện cho các lệnh API này, như vô hiệu hóa ghi log S3 và cài đặt chặn truy cập công khai của S3.
![VPC](/images/6/6.2/s4.png)
#### Khắc phục thủ công S3 bucket
Bạn đã xác nhận với chủ sở hữu bucket rằng cần khôi phục lại các thay đổi. Tiếp tục khắc phục các phát hiện S3.


5. Điều hướng đến  [bảng điều khiển AWS S3](https://s3.console.aws.amazon.com/s3/home?region=us-east-1).


6. Nhấp vào tên của S3 bucket bắt đầu bằng "guardduty-example-finance".



7. Nhấp vào tab Properties, cuộn xuống Server access logging, nhấp Edit và chọn Enable.


8. Sau khi chọn Enable, một lời nhắc để xác định "Target bucket" sẽ xuất hiện. Nhấp vào Browse S3 và chọn tên bucket bắt đầu bằng guardduty-example-log. Nhấp vào Choose path.


9. Cuối cùng, nhấp Save changes. Bạn đã bật ghi log truy cập máy chủ!


10. Để khôi phục bucket về trạng thái riêng tư, nhấp vào tab Permissions, và nhấp vào nút Edit dưới Block public access (bucket setting).


11. Chọn Block all public access, nhấp vào Save changes, và nhập "confirm" vào hộp thoại xuất hiện. Nhấp vào Confirm. TBucket đã trở lại trạng thái riêng tư! Một tùy chọn khác để khách hàng bảo vệ chống lại việc vô tình tiết lộ bucket là thêm chính sách điều khiển dịch vụ (service control policies - SCP). AMột SCP định nghĩa một giới hạn, hoặc đặt ra các giới hạn, về các hành động mà quản trị viên tài khoản có thể ủy quyền cho các user và IAM role trong tài khoản.


#### Vô hiệu hóa các khóa truy cập đã được xác định trong sự cố
Trong khi đội bảo mật đang phân tích hoạt động trước đây của người dùng này để hiểu rõ hơn về phạm vi của vấn đề, bạn cần phải vô hiệu hóa khóa truy cập liên quan đến người dùng để ngăn chặn các hành động không được ủy quyền hoặc không mong muốn khác.

12. Điều hướng đến bảng điều khiển AWS IAM.


13. Nhấp vào Users trong thanh điều hướng bên trái.



14. Chọn GD-Workshop-Compromised2-Simulated user.


15. Nhấp vào tab Security Credentials.



16. Cuộn xuống Access keys, nhấp vào dropdown Actions, và chọn Deactivate. Trong cửa sổ xác nhận bật lên, nhấp vào Deactivate. Nếu bạn gặp lỗi khi làm cho khóa không hoạt động, bạn có thể xóa khóa truy cập.


#### Tự động hóa việc khắc phục
Trong bước 7, bạn đã khắc phục thủ công một phần vấn đề bằng cách bật lại tính năng ghi log truy cập máy chủ S3. Bạn cũng có tùy chọn thực hiện điều này bằng cách tự động hóa. Một cách tiếp cận là sử dụng AWS Config Managed Rules. AWS Config cung cấp các rule được quản lý bởi AWS, là những rule được định sẵn và có thể tùy chỉnh mà AWS Config sử dụng để đánh giá liệu các tài nguyên AWS của bạn có tuân thủ các phương pháp tốt nhất hay không. Bạn có thể tùy chỉnh hành vi của một rule được quản lý để phù hợp với nhu cầu của bạn. AWS Config cho phép bạn khắc phục các tài nguyên không tuân thủ được đánh giá bởi AWS Config Rules. Trong trường hợp này, bạn có thể sử dụng rule được quản lý "s3-bucket-logging-enabled" và cấu hình rule để tự động bật ghi log cho các S3 buckets của bạn.