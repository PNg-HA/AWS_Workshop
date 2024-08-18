---
title : "Tổng hợp tìm kiếm qua các Region"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4. </b> "
---
Với việc tổng hợp qua các Region, bạn có thể tổng hợp các kết quả tìm kiếm, cập nhật tìm kiếm, thông tin chi tiết, trạng thái tuân thủ điều khiển và điểm số bảo mật từ nhiều Region đến một Region tổng hợp duy nhất. Sau đó, bạn có thể quản lý tất cả dữ liệu này từ Region tổng hợp.

Giả sử bạn thiết lập Region tổng hợp là US East (N. Virginia) và các Region liên kết là US West (Oregon) và US West (N. California). Khi bạn xem trang Findings ở US East (N. Virginia), bạn sẽ thấy các kết quả tìm kiếm từ cả ba Region. Các cập nhật đối với các kết quả tìm kiếm đó cũng sẽ được phản ánh ở tất cả ba Region. Trạng thái kích hoạt của một kiểm soát phải được thay đổi ở mỗi Region. Nếu một kiểm soát được kích hoạt ở một Region liên kết nhưng bị vô hiệu hóa ở Region tổng hợp, bạn có thể xem trạng thái tuân thủ của kiểm soát đó từ Region tổng hợp, nhưng không thể kích hoạt hoặc vô hiệu hóa kiểm soát đó từ Region tổng hợp.

#### Thiết lập tổng hợp tìm kiếm qua các Region
1. Bạn có thể đã thấy một thông báo để **Configure finding aggregation**. Bạn có thể chọn bất kỳ thông báo nào để tiếp tục thiết lập. Nếu bạn không để ý đến điều này, hãy mở trang **Regions** dưới phần Settings từ thanh điều hướng bên trái.



2. Chọn **Configure finding aggregation**.
![VPC](/images/3/3.4/s2.png)


3. Chọn **US East (N. Virginia) - us-east-1** làm Region Tổng hợp của bạn.



4. Chọn tất cả các Region có sẵn.



6. Chọn **Save** ở dưới cùng của trang. Điều này sẽ kích hoạt tổng hợp qua các Region của các kết quả tìm kiếm, nhưng sẽ không thực sự bật Security Hub ở các Region khác.
![VPC](/images/3/3.4/s6.png)

**Lưu ý cho độc giả**: Khi bạn nhấp Save để kích hoạt tổng hợp qua các Region, nó sẽ xóa các điểm số tổng hợp và dữ liệu điều khiển trong Security Hub. Nó không xóa các kết quả tìm kiếm hoặc ngừng tạo các kết quả tìm kiếm, nhưng bạn sẽ nhận thấy rằng các điểm số sẽ không xuất hiện (ví dụ, trên trang Summary) cho đến khi các điểm số mới được tính toán dựa trên nhiều Region. Bạn có thể thực hiện phần của workshop có tiêu đề "Security Posture Management" trước khi hoàn thành mô-đun này.


#### Bật các dịch vụ bảo mật AWS bạn sử dụng trong Region liên kết
Mặc dù nhiều dịch vụ bảo mật AWS đã được bật ở us-east-1 (N. Virginia) và bạn đã thiết lập tổng hợp kết quả tìm kiếm trong Security Hub, điều đó không có nghĩa là tất cả các dịch vụ đó đã được bật ở các Region khác mà bạn dự định hoạt động. Để nhanh chóng bật tất cả các dịch vụ bảo mật AWS giống nhau ở us-east-2 (Ohio), hãy sử dụng CloudFormation để triển khai mọi thứ ở Region thứ hai. Ngay khi các dịch vụ đó được bật ở Region thứ hai, các kết quả tìm kiếm từ chúng sẽ tự động được tổng hợp vào Security Hub ở us-east-1 (N. Virginia) nhờ vào thiết lập bạn đã thực hiện ở trên.

7. Mở trang https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/?filteringText=cfn&filteringStatus=active&viewNested=false trong bảng điều khiển CloudFormation.

![VPC](/images/3/3.4/s7.png)

8. Chọn tên của stack, **cfn**.
![VPC](/images/3/3.4/s8.png)


9. Mở tab **Outputs**.
![VPC](/images/3/3.4/s9.png)


10. Chọn liên kết "SecurityServicesCloudFormationLink" để khởi chạy một mẫu CloudFormation được cấu hình sẵn ở us-east-2 (Ohio). Điều này sẽ mở trang "Quick create stack" ở us-east-2 (Ohio) và điền thông tin và tham số stack. Đảm bảo rằng nó hiển thị "Ohio" ở góc trên bên phải của trang.



11. Để các giá trị giữ nguyên cho "Stack name", "MyAssetsBucketName", và "MyAssetsBucketPrefix". Nếu không có giá trị nào, hãy hỏi người hướng dẫn của bạn.
![VPC](/images/3/3.4/s11.png)


12.  Ở dưới cùng của trang, đánh dấu vào ô bên cạnh **I acknowledge that AWS CloudFormation might create IAM resources with custom names and I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**.



13. Chọn Create stack. Điều này sẽ bật AWS Config, Security Hub, Inspector, Detective, Macie và GuardDuty ở us-east-2 (Ohio). Bởi vì bạn đã cấu hình tổng hợp kết quả tìm kiếm trong Security Hub, ngay khi các kết quả tìm kiếm từ bất kỳ dịch vụ nào trong số này xuất hiện trong Security Hub ở Region này, chúng sẽ cũng xuất hiện trong Region tổng hợp của bạn. Điều này sẽ mất vài phút. Bạn có thể tiếp tục mà không cần chờ đợi.
![VPC](/images/3/3.4/s13.png)


14. Quay lại Security Hub. Nếu bạn không chuyển lại về Region workshop gốc, bạn sẽ thấy một thông báo để Đi đến Region tổng hợp. Chọn thông báo đó. Đảm bảo rằng bạn đã trở lại us-east-1 (N. Virginia) (bạn nên thấy "N. Virginia" ở góc trên bên phải của màn hình).



15. Khi bạn đã quay lại Security Hub trong Region tổng hợp của bạn, bạn sẽ nhận thấy rằng trang Findings hiện bao gồm các kết quả tìm kiếm từ nhiều Region khác nhau, và các hình ảnh trên trang Summary cũng làm nổi bật các kết quả tìm kiếm qua các Region. Chúng ta sẽ khám phá các trang đó sau.

(Sau 5 phút)
![VPC](/images/3/3.4/s15.png)