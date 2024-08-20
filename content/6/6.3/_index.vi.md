---
title : "Ứng phó khi thông tin xác thực IAM bị xâm phạm"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 6.3 </b> "
---

{{%notice info%}}
Tình huống / Báo cáo vấn đề
Bạn được giao nhiệm vụ điều tra và khắc phục một phát hiện từ GuardDuty. Loại phát hiện của GuardDuty là "UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom".
{{%/notice%}}

1. Điều hướng đến [bảng điều khiển GuardDuty](https://us-east-1.console.aws.amazon.com/guardduty/home?region=us-east-1#/findings) và mở trang Findings. Bạn sẽ thấy một phát hiện bắt đầu với "UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom". Nếu bạn không thấy, hãy thử làm mới trang.
![VPC](/images/6/6.3/s1.png)


2. Chọn một phát hiện loại "UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom"  bằng cách nhấp vào hàng của phát hiện đó. Phát hiện này thông báo rằng một API operation (ví dụ: khởi chạy một EC2 instance, tạo một IAM user mới, sửa đổi quyền hạn AWS) đã được thực hiện từ một địa chỉ IP nằm trong danh sách đe dọa mà bạn đã tải lên. Danh sách đe dọa bao gồm các địa chỉ IP độc hại đã biết. Điều này có thể chỉ ra truy cập trái phép vào tài nguyên AWS trong môi trường của bạn. Là một phần của chiến lược phát hiện dựa trên rủi ro, tổ chức của bạn ưu tiên các cảnh báo bảo mật liên quan đến IAM của AWS.
![VPC](/images/6/6.3/s2.png)


3. Bạn có thể thấy rằng Access Key được tham chiếu trong phát hiện này là từ một IAM assumed role, điều này có nghĩa là thông tin xác thực Access Key được sử dụng để thực hiện các lệnh gọi API thuộc về một IAM Role được giả định bởi một instance EC2. Trong phần Instance details trong chi tiết phát hiện, bạn sẽ có thể xác định ID instance của EC2 instance liên quan, cũng như IAM role ("User name") và Access Key được sử dụng. Ghi lại ID instance. ID instance bắt đầu bằng ‘i’ (such as i-08faaaafb15777f5a). Chúng ta sẽ cần điều này sau để xác định cách thức thông tin xác thực bị xâm phạm.
![VPC](/images/6/6.3/s3.png)

4. Vẫn trong phần Resources affected trong chi tiết phát hiện, ghi lại tên User name được gán cho  instance. Đây là IAM Role mà Access Key thuộc về.


5. Cuối cùng, vẫn trong phần Resources affected trong chi tiết phát hiện, ghi lại Access Key ID.



6. Ở đầu trang Findings, nhấp vào thanh Add filter để thêm tiêu chí lọc. Chọn Access Key ID, dán Access Key của bạn vào ô.


7. Nhấp vào Apply. Điều này sẽ hiển thị tất cả các phát hiện của GuardDuty liên quan đến Access Key đó. Nếu có các phát hiện khác, hãy dành chút thời gian để điều tra chúng.

{{%notice warning%}}
Trước khi tiếp tục, hãy đảm bảo bạn đã ghi lại ID instance, IAM role ("User name") và Access Key ID. Bạn sẽ cần những giá trị này sau trong module này.
{{%/notice%}}
![VPC](/images/6/6.3/s7.png)

#### Phản hồi khi thông tin xác thực IAM của AWS bị xâm phạm
8. Bây giờ bạn đã xác định được rằng kẻ tấn công đang sử dụng thông tin xác thực bảo mật tạm thời từ IAM role cho EC2, quyết định đã được đưa ra là luân phiên thông tin xác thực đó ngay lập tức nhằm ngăn chặn mọi hành vi sử dụng sai mục đích hoặc leo thang đặc quyền tiềm ẩn (potential privilege escalation). Để thu hồi các phiên IAM role, chúng ta cần truy cập bảng điều khiển AWS Identity and Access Management (IAM). https://console.aws.amazon.com/iamv2/home 


9. Nhấp vào Roles bên trái và tìm tên role mà bạn đã xác định trong phần trước bằng cách sử dụng tên User name mà bạn đã ghi lại trước đó (đây là IAM Role gắn với instance bị xâm phạm). Nếu bạn không thấy nó, bạn có thể sử dụng trường tìm kiếm dưới Roles. Nhấp vào tên Role đó.

![VPC](/images/6/6.3/s9.png)

10. Nhấp vào tab Revoke sessions.
![VPC](/images/6/6.3/s10.png)
Sau đó, không thể truy cập vào instance.
![VPC](/images/6/6.3/s10b.png)
11. Nhấp vào Revoke active sessions.
![VPC](/images/6/6.3/s11.png)

12.  Chọn ô xác nhận và sau đó nhấp vào Revoke active sessions. Bạn sẽ thấy một thông báo xuất hiện ở đầu trang  "Active sessions have been successfully revoked".
![VPC](/images/6/6.3/s12.png)
#### Khởi động lại EC2 instance để luân phiên access key
Tất cả thông tin xác thực đang hoạt động cho IAM role bị xâm phạm đã bị vô hiệu hóa. Điều này có nghĩa là kẻ tấn công không còn có thể sử dụng các Access Key đó, nhưng cũng có nghĩa là bất kỳ ứng dụng nào sử dụng role này cũng không thể. Bạn đã biết điều này trước khi tiếp tục nhưng đã quyết định rằng nó là cần thiết do rủi ro cao của một Access Key cập IAM bị xâm phạm. Để đảm bảo tính khả dụng của ứng dụng của bạn, bạn cần làm mới các Access Key trên instance bằng cách dừng và khởi động lại instance. Khởi động lại đơn giản sẽ không thay đổi các key. Nếu bạn chờ, thông tin xác thực bảo mật tạm thời trên instance sẽ được làm mới nhưng quy trình này sẽ đẩy nhanh việc thay đổi key. Vì bạn đang sử dụng AWS Systems Manager để quản lý các instance EC2 của mình, bạn có thể sử dụng nó để truy vấn metadata để xác thực rằng các Access Key đã được luân phiên sau khi khởi động lại instance.

13.  Mở bảng điều khiển Elastic Cloud Compute (EC2) console và điều hướng đến trang Instances. https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running 


14. Tìm ID instance mà bạn đã ghi lại trước đó trong bước 3. Chọn ô bên cạnh instance đó.



15. Sau đó nhấp vào menu thả xuống Instance state, và chọn Stop instance.


16. Một hộp thoại xác nhận sẽ xuất hiện. Nhấp vào Stop một lần nữa.


17. Chờ Instance State chuyển sang trạng thái stopped dưới Instance State (bạn có thể cần làm mới bảng điều khiển EC2). Bạn có thể cần loại bỏ bộ lọc "running" trong danh sách các instance của mình để thấy instance được liệt kê khi nó đã dừng lại.


18. Chọn ô bên cạnh instance và làm theo các bước tương tự để khởi động lại instance.


#### Xác minh Access Key đã được luân phiên

19. Truy cập AWS Systems Manager và mở trang Session Manager. https://us-east-1.console.aws.amazon.com/systems-manager/session-manager?region=us-east-1# 


20. Nhấp vào Start Session bên phải.



21. Chọn ID instance từ phần trước (mà bạn đã dừng và khởi động lại) bằng cách nhấp vào radio button bên cạnh nó. Sau đó nhấp vào Start session. Điều này sẽ mở một shell (một phiên với instance đó).


22. Chạy hai lệnh sau và so sánh Access Key ID với key mà bạn đã ghi lại trước đó để đảm bảo nó đã thay đổi. Hãy chắc chắn rằng bạn thay thế từ "ROLE" ở cuối lệnh thứ hai bằng "User name" hoặc IAM role mà bạn đã ghi lại trong bước 4.

```
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/
```

```
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/ROLE
```

23. So sánh AccessKeyId trong phản hồi với Access Key ID bạn đã ghi lại trước đó để xác nhận việc luân phiên thông tin xác thực đã thành công (chúng phải khác nhau).

Trả lời: Khác với AccessKeyID cũ ASIAXCSEC6TNGZ6EZUUO:
![VPC](/images/6/6.3/s23.png)
{{%notice tip%}}
Để tìm hiểu thêm, hãy tham khảo ví dụ về Incident Response Playbook for Credential Leakage/Compromise trên AWS Samples: https://github.com/aws-samples/aws-incident-response-playbooks/blob/master/playbooks/IRP-CredCompromise.md 
{{%/notice%}}

#### Điều tra phiên role bị xâm phạm với Amazon Detective
Bây giờ bạn đã xem xét và khắc phục phát hiện của GuardDuty "UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom", hãy tiếp tục điều tra những gì đã xảy ra với tài nguyên này và tìm kiếm bất kỳ hoạt động đáng ngờ nào khác liên quan. Bạn có thể chọn điều tra với Detective trước hoặc sau các bước trước đó..

24. Quay lại Amazon GuardDuty và mở lại phát hiện tương tự. Tham khảo các bước trước nếu bạn quên cách mở phát hiện, UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom.


25. Ở đầu bảng tổng quan, nhấp vào liên kết có nhãn Investigate with Detective.


26. Trong cửa sổ pop-up Investigate with Detective, chọn Role session.
![VPC](/images/6/6.3/s26.png)

27. Bây giờ bạn sẽ ở trang Role session trong bảng điều khiển của Detective với chi tiết về phát hiện UnauthorizedAccess:IAMUser/MaliciousIPCaller.Custom ở phía bên phải màn hình. Tiến hành trả lời một số câu hỏi để giúp bạn xác định nguyên nhân cốt lõi trong cuộc điều tra.
![VPC](/images/6/6.3/s27.png)

![VPC](/images/6/6.3/s27b.png)
#### Điều này xảy ra khi nào?
28.  Bảng đầu tiên trong tổng quan là Role session details. Bảng này cung cấp thông tin tổng quan về phiên role liên quan đến phát hiện của GuardDuty, bao gồm tài khoản liên quan, role được giả định và thời gian được quan sát lần đầu và cuối cùng.


29. Xem xét khi nào phiên role được quan sát lần đầu và lần cuối. Thông tin này có tương ứng với thời điểm phát hiện của GuardDuty được tạo và cập nhật không? Thông tin này có thể giúp bạn thu hẹp khoảng thời gian mà role này bị xâm phạm.

Thời gian của GuardDuty và Detective không giống nhau (thời gian trong bảng điều khiển GuardDuty là thời gian trên máy tính xách tay của bạn, còn thời gian trong Detective là UTC). Tuy nhiên, trong file json của phát hiện GuardDuty, thời gian này giống như trong Detective.
![VPC](/images/6/6.3/s29.png)

Bắt đầu từ 17:44 UTC ngày 17/07/2024
![VPC](/images/6/6.3/s29b.png)

#### Điều này xảy ra trong tài khoản nào và role được giả định có những quyền gì?
30.  Lưu ý tài khoản AWS trong phần Role session details. Bạn nên kiểm tra xem tài khoản này có chứa dữ liệu nhạy cảm hoặc công việc sản xuất hay không bằng cách tra cứu trong cơ sở dữ liệu quản lý cấu hình (configuration management database - CMBD) hoặc công cụ khác mà bạn sử dụng để theo dõi.



31. Tìm liên kết dưới Assumed role trong Role session details và mở nó trong một tab mới. Xem xét các phát hiện liên quan đến role này.



32. Ở đầu trang, tìm liên kết AWS role dưới Role details. Mở liên kết này trong một tab mới. Điều này sẽ đưa bạn đến bảng điều khiển IAM.


33. Từ bảng điều khiển IAM, bạn sẽ có thể xác định quyền hạn của role, xem có các thẻ nào giúp bạn xác định role này được sử dụng cho mục đích gì và cung cấp cho bạn một điểm pivot trực tiếp từ Detective để thu hồi các phiên.


34. Bạn có thể đóng tab bảng điều khiển IAM. Quay lại tab với Detective đang mở đến AwsRoleSession. Ở đầu trang, bạn sẽ thấy Detective > Search > AwsRoleSession.
![VPC](/images/6/6.3/s34.png)

#### Những phát hiện nào khác liên quan đến phiên role này?
35. Xem phần Findings associated with this resource. Xem xét các phát hiện theo thứ tự chúng được quan sát.
![VPC](/images/6/6.3/s35.png)

#### Có sự tăng đột biến bất thường nào trong các lệnh gọi API thành công hoặc thất bại không?
36. Cuộn xuống bảng Overall API call volume.
![VPC](/images/6/6.3/s36.png)

37. Nhấp vào Display details for scope time để xem thêm chi tiết.

Kết quả: 
![VPC](/images/6/6.3/s37.png)
38. Dưới Successful calls, nhấp vào một trong các thanh của lệnh gọi API vượt quá mức cơ sở (đường màu xanh). Sau đó nhấp vào Set time interval. Điều này sẽ cập nhật Activity for time window.


39. Dưới Activity for time window, mở rộng các menu thả xuống để xem địa chỉ IP nào đã thực hiện các lệnh gọi API nào. Bạn cũng có thể chuyển sang các tab API method by service và Access Key ID.


40. Dưới Failed calls, nhấp vào một trong các thanh của lệnh gọi API thất bại. Sau đó nhấp vào Set time interval. Điều này sẽ cập nhật Activity for time window. Xem xét các lệnh gọi API đã thất bại.



41. Bạn có thể điều chỉnh Scope time ở đầu trang để xác định xem có hoạt động đáng ngờ nào liên quan đến role này trước hoặc sau khi phát hiện GuardDuty không.

#### Có bất kỳ hành vi mới nào được phát hiện không?

{{%notice tip%}}
Một hướng điều tra cho một phát hiện là so sánh hoạt động trong thời gian phạm vi phát hiện với hoạt động đã xảy ra trước khi phát hiện được phát hiện. Hoạt động chưa từng thấy trước đây có thể có khả năng đáng ngờ hơn.\
Một số bảng hồ sơ của Amazon Detective làm nổi bật hoạt động không được quan sát thấy trong thời gian trước phát hiện. Một số bảng hồ sơ cũng hiển thị giá trị cơ sở để cho thấy hoạt động trung bình trong 45 ngày trước thời gian phạm vi. Thời gian phạm vi là tóm tắt hoạt động của một thực thể theo thời gian.\
Khi nhiều dữ liệu hơn được trích xuất vào biểu đồ hành vi của bạn, Detective sẽ phát triển một bức tranh chính xác hơn về hoạt động nào là bình thường trong tổ chức của bạn và hoạt động nào là bất thường.\
Tuy nhiên, để tạo ra bức tranh này, Detective cần truy cập dữ liệu ít nhất hai tuần. Mức độ trưởng thành của phân tích Detective cũng tăng lên với số lượng tài khoản trong biểu đồ hành vi.\
Hai tuần đầu tiên sau khi bạn kích hoạt Detective được coi là giai đoạn huấn luyện. Trong giai đoạn này, các bảng hồ sơ so sánh hoạt động thời gian phạm vi với hoạt động trước đó hiển thị một thông báo rằng Detective đang trong giai đoạn huấn luyện.\
Trong thời gian dùng thử, Detective khuyến nghị bạn thêm càng nhiều tài khoản thành viên càng tốt vào biểu đồ hành vi. Điều này cung cấp cho Detective một lượng dữ liệu lớn hơn, cho phép nó tạo ra một bức tranh chính xác hơn về hoạt động bình thường cho tổ chức của bạn.
{{%/notice%}}


42.  Ở đầu trang, chuyển sang tab New behavior. Mặc dù bạn sẽ thấy một số thông báo trên trang này cho biết "Panel in training", hãy dành vài phút để xem xét các hành vi mới, bao gồm "Newly observed geolocations", "Newly observed API calls", "API calls with increased volume", "Newly observed autonomous system organizations", và "Newly observed user agents".
![VPC](/images/6/6.3/s42.png)