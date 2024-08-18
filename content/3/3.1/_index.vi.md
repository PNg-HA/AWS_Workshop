---
title : "Tập trung các phát hiện từ các dịch vụ bảo mật AWS"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1. </b> "
---

Security Hub tự động thu thập và hợp nhất các phát hiện từ các dịch vụ bảo mật AWS được kích hoạt trong môi trường của bạn, chẳng hạn như các phát hiện xâm nhập từ Amazon GuardDuty, quét lỗ hổng từ Amazon Inspector, phát hiện Amazon Simple Storage Service (Amazon S3) bucket policy của Amazon Macie, tài nguyên công khai và tài nguyên giữa các tài khoản từ IAM Access Analyzer, và các tài nguyên thiếu sự bảo vệ từ AWS Firewall Manager.


Việc tập hợp này cung cấp cho bạn cái nhìn tổng quan về tình trạng bảo mật và tuân thủ trong môi trường AWS của bạn. AWS Security Hub hợp nhất các phát hiện bảo mật từ hàng chục dịch vụ bảo mật AWS và sản phẩm APN tại một nơi duy nhất và theo một định dạng duy nhất là AWS Security Finding Format (ASFF). Security Hub không phát hiện và hợp nhất các phát hiện bảo mật được tạo ra trước khi bạn kích hoạt Security Hub. Tất cả các phát hiện được lưu trữ trong Security Hub trong 90 ngày sau ngày cập nhật cuối cùng.

Trang Tích hợp (Integrations) trong AWS Management Console cung cấp quyền truy cập vào tất cả các tích hợp sản phẩm của AWS và bên thứ ba có sẵn. API của AWS Security Hub cũng cung cấp các thao tác cho phép bạn quản lý các tích hợp. Đối với các dịch vụ AWS, Security Hub tự động kích hoạt tích hợp, và bạn có thể tùy chọn vô hiệu hóa từng tích hợp.

#### Tập trung các phát hiện từ các dịch vụ bảo mật AWS
1. Điều hướng đến Security Hub và mở trang Summary. Trang [Security Hub Summary page](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/summary) cung cấp cho bạn cái nhìn tổng quan về tình trạng bảo mật và tuân thủ của tài khoản AWS của bạn.


2. Dành vài phút để xem lại các thông tin tổng quan được tạo ra trên trang Summary. Bạn có thể tìm thấy các loại mối đe dọa phổ biến nhất được xác định trong môi trường của mình không? Các phát hiện này đang được tập hợp trong Security Hub từ Amazon GuardDuty.
Security Hub widgets
![VPC](/images/3/3.1/s2.png)
3. Mở trang Tích hợp (Integrations) trong Security Hub. Cuộn qua các tích hợp có sẵn.
![VPC](/images/3/3.1/s3a.png)
![VPC](/images/3/3.1/s3b.png)
Như bạn có thể thấy, nhiều tích hợp, bao gồm cả "Amazon: GuardDuty" và "Amazon: Inspector", được tự động kích hoạt cho bạn. Tuy nhiên, bạn vẫn cần kích hoạt từng dịch vụ (ngoài tích hợp) để tạo ra các phát hiện tương ứng.

4. Security Hub cung cấp cho bạn sự linh hoạt để kích hoạt và vô hiệu hóa các tích hợp AWS. Mặc dù không thường được khuyến nghị, hãy thử vô hiệu hóa tích hợp AWS: Firewall Manager. Chọn Stop accepting findings trong ô được gán nhãn AWS: Firewall Manager.
![VPC](/images/3/3.1/s5a.png?featherlight=false&width=10pc)
5. Bạn sẽ được hiển thị một thông báo bật lên rằng "Not accepting findings from this integration means that the product can still try to send findings to Security Hub, but Security Hub will not ingest them." Chọn Cancel. Chúng ta nên để tích hợp này bật.
![VPC](/images/3/3.1/s5b.png)
6. Để xem tất cả các phát hiện bảo mật từ một dịch vụ cụ thể, bạn có thể sử dụng trang này để xem. Tìm tích hợp cho "Amazon: GuardDuty" và chọn See findings. iều này sẽ mở trang phát hiện và thêm một bộ lọc để bạn chỉ xem các phát hiện từ Amazon GuardDuty. Ở lại trên trang Findings.

7.  Bạn có thể tùy chọn xóa hoặc thay đổi một số bộ lọc để điều chỉnh danh sách các phát hiện đã lọc mà bạn đang xem.

#### Tìm kiếm và xem lại các phát hiện bảo mật
8. Để xem tất cả các phát hiện trong Security Hub, hãy xóa các bộ lọc (và Group By, nếu có) ở đầu trang bằng cách chọn dấu X bên cạnh mỗi bộ lọc.

9. Có rất nhiều phát hiện Security Hub được liệt kê ở đây. Hãy thử thêm một bộ lọc để thu hẹp danh sách xuống chỉ còn các phát hiện có mức độ nghiêm trọng cao từ dịch vụ phát hiện mối đe dọa, GuardDuty. Chọn  Add filters trong thanh tìm kiếm.

10. Từ menu thả xuống, chọn nhãn Severity label và nhập vào HIGH. Chú ý viết hoa. Chọn Apply.
![VPC](/images/3/3.1/s10a.png)
Kết quả:
![VPC](/images/3/3.1/s10b.png)
Chọn một phát hiện để xem thêm thông tin.
![VPC](/images/3/3.1/s10c.png)
11. Chọn Add filters trong thanh tìm kiếm lần nữa. Lần này, chọn Product name và nhập vào GuardDuty. Chọn Apply.

12. Chọn một trong các phát hiện và chọn tiêu đề của nó. Điều này sẽ mở ra bảng chi tiết phát hiện. Mở rộng tất cả các phần và dành vài phút để xem lại thông tin ở đây. Bạn có thể thấy mô tả, một liên kết đến hướng dẫn khắc phục, thông tin về tài nguyên, và nhiều hơn nữa.



13. Trong chi tiết của phát hiện, chọn ID của phát hiện ở đầu để hiển thị toàn bộ JSON cho phát hiện đó. Security Hub xử lý các phát hiện bằng cách sử dụng định dạng phát hiện tiêu chuẩn gọi là AWS Security Finding Format - ASFF, giúp loại bỏ việc phải chuyển đổi dữ liệu tốn thời gian. JSON của phát hiện có thể được tải xuống thành tệp nếu cần cho việc điều tra thêm.
![VPC](/images/3/3.1/s13.png)

14. Đóng cửa sổ bật lên JSON bằng cách chọn dấu X ở góc trên bên phải.

15. Ở đầu chi tiết phát hiện, mở tab History tab để xem danh sách theo thứ tự thời gian của tất cả các thay đổi đã được thực hiện đối với phát hiện. Sự minh bạch của lịch sử phát hiện giúp bạn nhanh chóng nhận diện các rủi ro bảo mật tiềm ẩn và thực hiện các bước chủ động để giảm thiểu chúng.

16. Đóng chi tiết phát hiện bằng cách chọn dấu X ở góc trên bên phải, nhưng vẫn ở lại trang phát hiện.

17. Hãy thử xem các tài nguyên nào trong môi trường của chúng ta đang tạo ra nhiều phát hiện nhất. Loại bỏ tất cả các bộ lọc. Sau đó thêm một bộ lọc mới, chọn Resource type sau đó nhập AwsAccount. Chọn Apply.
![VPC](/images/3/3.1/s17a.png)
Kết quả:
![VPC](/images/3/3.1/s17b.png)
18. Thêm một bộ lọc khác.Chọn Record state, sau đó nhập ACTIVE. Chọn Apply.
![VPC](/images/3/3.1/s18.png)
19. Chọn Add filters trong thanh tìm kiếm lần nữa. Lần này, chọn Group by và chọn ResourceId. Chọn Apply. Danh sách này cho bạn biết số lượng phát hiện bảo mật trên mỗi tài nguyên. Điều này có thể giúp bạn ưu tiên công việc dựa trên các tài nguyên trong môi trường của bạn có nhiều vấn đề bảo mật nhất.
![VPC](/images/3/3.1/s19.png)
Kết quả:
![VPC](/images/3/3.1/s19b.png)
#### Generating insights from aggregated findings
20. Nếu bạn thấy danh sách số lượng phát hiện bảo mật trên mỗi tài nguyên có lợi, bạn có thể lưu nó thành một Insight tùy chỉnh để tham khảo trong tương lai. Chọn Create insight ở góc trên bên phải của màn hình.

21. Đặt tên cho Insight là "0. Resources by counts of findings". Sau đó chọn Create insight.
![VPC](/images/3/3.1/s20.png)
22. Trong điều hướng bên trái, chọn Insights.  Một Insight của Security Hub là một tập hợp các phát hiện liên quan được xác định bằng một câu lệnh tổng hợp và các bộ lọc tùy chọn. Một Insight xác định một khu vực bảo mật cần chú ý và can thiệp. Security Hub cung cấp một số Insight được quản lý (mặc định) mà bạn không thể sửa đổi hoặc xóa. Bạn cũng có thể tạo các Insight tùy chỉnh để theo dõi các vấn đề bảo mật độc đáo với môi trường và cách sử dụng AWS của bạn.

23. Insight đầu tiên được liệt kê là Insight bạn vừa tạo. Chọn tiêu đề 0. Resources by counts of findings. Tại đây, bạn có cùng một giao diện như trước cộng với các biểu đồ được tạo ra. Chúng ta sẽ tìm hiểu sâu hơn về Insights sau.
![VPC](/images/3/3.1/s23.png)
24.  Quay lại trang Insights bằng cách sử dụng thanh điều hướng bên trái.

25. Dành vài phút để xem qua các Insights được AWS cung cấp. Lọc theo mức độ nghiêm trọng của Insight.

26. Chọn 24. Severity by counts of findings.

27. Chọn một nhãn Severity (e.g. Critical) (ví dụ: Critical) để xem các phát hiện cơ bản.

![VPC](/images/3/3.1/s26.png)
Kết quả:
![VPC](/images/3/3.1/s26b.png)