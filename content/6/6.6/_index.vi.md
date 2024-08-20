---
title : "Ứng phó với mã độc trên Amazon Elastic Block Store"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 6.5 </b> "
---
{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Trong mô-đun trước, "Building your own automated response", bạn đã khám phá các phương pháp phản hồi tự động. Mô-đun này liên quan, nhưng thay vì tập trung vào phản hồi tự động, nhiệm vụ của bạn là điều tra một instance EC2 có thể bị xâm nhập liên quan đến một phát hiện UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom" từ GuardDuty. Để thực hiện điều này, bạn sẽ sử dụng Amazon Detective.
{{%/notice%}}

1. Mở Amazon GuardDuty và chọn phát hiện có tiêu đề UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom to open the finding details.
![VPC](/images/6/6.6/s1.png)

2. Ở đầu bảng tổng quan, nhấp vào liên kết có nhãn Investigate with Detective.


3. Trong cửa sổ Investigate with Detective xuất hiện, chọn EC2 Instance để điều tra EC2 instance bị xâm phạm.
![VPC](/images/6/6.6/s3.png)

4. Bây giờ, bạn sẽ ở trên trang thông tin entity của EC2 instance trong bảng điều khiển của Detective với chi tiết phát hiện UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom ở phía bên phải của màn hình. Tiến hành trả lời một số câu hỏi để đẩy nhanh quá trình điều tra nguyên nhân cốt lõi của bạn.


5. Lưu ý rằng phạm vi thời gian ở đầu trang được thiết lập từ khi sự kiện được phát hiện lần đầu tiên đến thời gian sự kiện được GuardDuty ghi nhận lần cuối. Điều này quan trọng vì phạm vi thời gian xác định tập dữ liệu mà Detective trả về cho bạn.
![VPC](/images/6/6.6/s5.png)

6. Xem thông tin được hiển thị ở phần trên cùng, chi tiết EC2 instance. AWS account, EC2 instance, Role, Associated VPC và các thông tin khác được liệt kê. Bạn có thể tùy chọn nhấp vào các liên kết để tìm hiểu thêm về các entity được liên kết. Ví dụ, bạn có thể theo dõi role được liên kết để hiểu quyền hạn của role được sử dụng bởi EC2 instance hoặc để xem liệu có các phát hiện khác liên quan đến role này không.




{{%notice tip%}}
Nếu bạn quan tâm đến việc khám phá thêm thông tin được trình bày trên trang này, bạn nên hoàn thành mô-đun có tiêu đề: Respond to compromised IAM credentials.
{{%/notice%}}


7. Cuộn xuống. Xem phần "Findings associated with this resource". Xem xét các phát hiện theo thứ tự mà chúng đã được quan sát.

![VPC](/images/6/6.6/s7.png)

8. Xem xét Overall VPC flow volume. Nhấp vào mức tăng đột biến trong lưu lượng Inbound và sau đó nhấp vào Set time interval. Xem lại Activity for time window. Điều này sẽ hiển thị dữ liệu lưu lượng VPC vào và ra khỏi instance EC2. Activity for time window sẽ tự động điều chỉnh thời gian theo phạm vi thời gian của cột và liệt kê địa chỉ IP, cổng, khối lượng lưu lượng vào và ra, giao thức và ghi chú xem lưu lượng đã được chấp nhận hay bị từ chối.
![VPC](/images/6/6.6/s8.png)
![VPC](/images/6/6.6/s8b.png)
9. Bạn cũng có thể kiểm tra "Distinct count of ports over time", "Distinct IP addresses over time", và "Observed IP address assignments based on VPC Flow".

![VPC](/images/6/6.6/s9.png)

10. Cuộn lên đầu trang entity của EC2 instance và chọn tab New behavior. Trong tab này, Detective phát triển một hình ảnh về hoạt động nào là bình thường trong tổ chức của bạn và hoạt động nào là bất thường. Cuộn qua các bảng khác nhau để xác định thông tin bạn có thể thu thập liên quan đến việc điều tra vấn đề bảo mật.


Kiểm tra một IP:
![VPC](/images/6/6.6/s10d.png)

Thiết lập khoảng thời gian để giám sát:
![VPC](/images/6/6.6/s10e_1.png)
![VPC](/images/6/6.6/s10e_2.png)
Thông tin chi tiết hơn:
![VPC](/images/6/6.6/s10e_3.png)

#### Tìm kiếm địa chỉ IP của tác nhân
11. Trong chi tiết phát hiện của GuardDuty ở bên trái, ghi lại địa chỉ IPv4 được liệt kê dưới phần Actor.
![VPC](/images/6/6.6/s11.png)

12. Mở trang Search từ bảng điều hướng ở phía bên phải.


13. Nhấp vào menu thả xuống Choose type và chọn IP address.


14. Nhập địa chỉ IP mà bạn đã ghi lại từ chi tiết phát hiện của GuardDuty. Nhấp vào Search.
![VPC](/images/6/6.6/s14.png)

15. Chọn liên kết địa chỉ IP để điều hướng đến trang entity của địa chỉ IP đó.
![VPC](/images/6/6.6/s15.png)

16. Giống như các bước trước, hãy dành thời gian để xem xét thông tin về địa chỉ IP. Tìm kiếm trên trang:

- Được quan sát lần đầu: 00:17 UTC
![VPC](/images/6/6.6/s16_first.png)

- Tổng số lần quan sát: Lần quan sát cuối – lần quan sát đầu
- Lần quan sát cuối là 18:42 UTC nên tổng thời gian là 25
![VPC](/images/6/6.6/s16_last.png)

Nhưng khác với tổng quan
![VPC](/images/6/6.6/s16b.png)

- Phát hiện liên quan đến tài nguyên này
![VPC](/images/6/6.6/s16_associate.png)

- Tổng số lượng lệnh gọi API
![VPC](/images/6/6.6/s16_overall.png)

17.   Ngoài ra, hãy khám phá các tab, New behavior, Resource interaction, và Kubernetes activity.

Tương tác tài nguyên: 
![VPC](/images/6/6.6/s17_rsc.png)
Hành vi mới:
![VPC](/images/6/6.6/s17_new.png)
![VPC](/images/6/6.6/s17_newb.png)
18. Trong tình huống này, bạn đã sử dụng Amazon Detective để tìm thông tin hỗ trợ điều tra vấn đề bảo mật với dữ liệu hạn chế. Trong môi trường sản xuất với nhiều dữ liệu hơn, bạn sẽ có thể sử dụng Detective hiệu quả hơn cho các hoạt động như hoạt động mới được quan sát, tương tác tài nguyên, và cơ sở đường truyền lưu lượng. Bây giờ bạn đã sử dụng Amazon Detective để thu thập thêm thông tin về mức độ của vấn đề bảo mật, bạn sẽ hiểu rõ hơn về các tài nguyên khác cần được khắc phục, quyền hạn cần được giảm bớt, và thông tin phân tích nguyên nhân cốt lõi để bao gồm trong báo cáo sau sự kiện.