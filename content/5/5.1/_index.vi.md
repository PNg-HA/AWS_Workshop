---
title : "Thiết lập thông báo"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---
Sau khi Security Hub đã thu nhận một phát hiện xác định mối đe dọa hoặc cho biết rằng có một cấu hình cần chú ý, bước tiếp theo là hành động và giải quyết phát hiện đó. Với Amazon EventBridge, bạn có thể tự động hóa các dịch vụ AWS của mình để phản hồi tự động với các sự kiện hệ thống như vấn đề về khả năng sẵn có của ứng dụng hoặc thay đổi tài nguyên.

Amazon EventBridge là một dịch vụ không máy chủ cho phép dễ dàng xây dựng các ứng dụng dựa trên sự kiện ở quy mô lớn bằng cách sử dụng các sự kiện được tạo từ các dịch vụ AWS. Các sự kiện từ các dịch vụ AWS được gửi đến EventBridge trong thời gian gần thực và trên cơ sở được bảo đảm. Bạn có thể viết các quy tắc đơn giản để chỉ định sự kiện nào mà bạn quan tâm và hành động tự động nào sẽ được thực hiện khi một sự kiện khớp với một quy tắc. Các hành động có thể được kích hoạt tự động bao gồm:

+ Gọi một hàm AWS Lambda
+ Kích hoạt một máy trạng thái AWS Step Functions
+ Thông báo một chủ đề Amazon SNS hoặc một hàng đợi Amazon SQS
+ Gửi phát hiện đến công cụ quản lý sự cố, chat, SIEM, hoặc phản hồi sự cố của bên thứ ba

Security Hub tự động gửi tất cả các phát hiện mới và tất cả các cập nhật cho các phát hiện hiện có tới EventBridge dưới dạng các sự kiện EventBridge. Bạn cũng có thể tạo các hành động tùy chỉnh cho phép bạn gửi các phát hiện đã chọn và kết quả tổng quan đến EventBridge. Sau đó, bạn cấu hình các quy tắc EventBridge để phản hồi với mỗi loại sự kiện.

Trong module này, chúng ta sẽ tạo một quy tắc EventBridge để tự động hóa thông báo trên danh sách các phát hiện đã được lọc từ Security Hub. Chúng ta sẽ cấu hình Amazon Simple Notification Service để nhận thông báo qua email khi chúng ta nhận được phát hiện có mức độ nghiêm trọng CAO hoặc NGHIÊM TRỌNG trong Security Hub. Amazon Simple Notification Service (Amazon SNS) là một dịch vụ nhắn tin được quản lý hoàn toàn cho cả giao tiếp giữa các ứng dụng (A2A) và giao tiếp giữa ứng dụng và người dùng (A2P).

Trong một kịch bản thực tế, ví dụ cụ thể này sẽ không hiệu quả trên quy mô lớn, nhưng nó sẽ minh họa cách AWS Security Hub hoạt động với Amazon EventBridge.

#### Cấu hình Amazon Simple Notification Service topic

1. Điều hướng đến [Amazon SNS](https://console.aws.amazon.com/sns/v3/home).

2. Nhấp vào **Topics** trong bảng điều hướng bên trái.

3. Nhấp vào **Create topic**.

4. Chọn **Standard** type.

5. Đặt tên là "high-severity-security-hub-findings". Kết quả:

6. Để nguyên mọi thứ khác và nhấp vào **Create topic** ở cuối trang. Điều này sẽ tạo chủ đề.
![VPC](/images/5/5.1/s6.png)


#### Đăng ký theo dõi topic

7. Từ trang **high-severity-security-hub-findings topic**, nhấp vào **Create subscription**.

8. Trên trang **Create subscription**, dưới **Protocol**, chọn **Email**.

9. Trên trang **Create subscription**, dưới **Endpoint**, nhập địa chỉ email mà bạn muốn sử dụng cho workshop này để nhận thông báo. Bạn có thể hủy đăng ký sau khi hoàn thành workshop.

10. Nhấp vào **Create subscription**.
![VPC](/images/5/5.1/s10.png)

11. Trong vài phút, bạn sẽ nhận được một email tại địa chỉ email bạn đã nhập. Xác nhận đăng ký bằng cách nhấp vào **Confirm subscription** trong email.



#### Tạo một EventBridge Rule để gửi findings đến chủ đề
Bây giờ chúng ta đã đăng ký vào **SNS topic**, chúng ta sẵn sàng gửi findings đến chủ đề. Để làm điều này, chúng ta sẽ tạo một **EventBridge rule** khớp với các sự kiện **Security Hub** cho các findings có mức độ nghiêm trọng **HIGH** và **CRITICAL**.

12. Điều hướng đến **Amazon EventBridge**. https://console.aws.amazon.com/events/home 

13. Nhấp vào **Create rule**.

14. Trên trang **Define rule detail**, đặt tên cho rule của bạn là "high-severity-security-hub-findings".

15. Nhấp vào **Next**.

16. Trên trang **Build event pattern**, trong phần **Event pattern**, nhấp vào **Edit pattern**.

17. Nhập **Event Pattern** sau:
![VPC](/images/5/5.1/s17.png)

```
{
  "source": ["aws.securityhub"],
  "detail": {
    "findings": {
      "Severity": {
      "Label": ["HIGH", "CRITICAL"]
      }
    }
  }
}
```

18. Nhấp vào **Next**.

19. Trên trang **Select target(s)**, từ danh sách thả xuống **Select a target**, chọn **SNS topic**.

20. Sau đó, từ danh sách thả xuống **Topic**, chọn **high-severity-security-hub-findings**.
![VPC](/images/5/5.1/s20.png)

21. Nhấp vào **Next**.

22. Trên trang **Configure tags - optional**, nhấp vào **Next**.

23. Trên trang **Review and create**, nhấp vào **Create rule**.
![VPC](/images/5/5.1/s23.png)


#### Kiểm tra thông báo
**EventBridge rule** mà chúng ta đã cấu hình sẽ đẩy một thông báo khi có bất kỳ cập nhật hoặc phát hiện mới trong **Security Hub** có nhãn mức độ nghiêm trọng "HIGH" hoặc "CRITICAL". Chúng ta có thể chờ một phát hiện mới hoặc cập nhật, nhưng sẽ nhanh hơn nếu chúng ta kiểm tra điều này bằng cách tự cập nhật một phát hiện. Một cách dễ dàng để làm điều này là thay đổi **Workflow status** của một phát hiện.

Đối với findings, **workflow status** theo dõi tiến trình điều tra của bạn đối với một phát hiện. **Workflow status** cụ thể cho từng phát hiện. Nó không ảnh hưởng đến việc tạo ra các phát hiện mới.

24. Quay lại [Security Hub](https://console.aws.amazon.com/securityhub).

25. Mở trang **Findings**.

26. Thêm một bộ lọc mới, với nhãn **Severity** là **HIGH**. Điều này có phân biệt chữ hoa chữ thường. Nhấp vào **Apply**.
![VPC](/images/5/5.1/s23.png)

27. Nhấp vào tiêu đề của bất kỳ phát hiện nào có mức độ nghiêm trọng cao để mở nó.

28. Dưới menu thả xuống **Workflow status** trong chi tiết phát hiện, thay đổi **workflow** từ **New** thành **Notified**.
![VPC](/images/5/5.1/s28.png)

29. Bạn sẽ thấy một biểu ngữ hiển thị "Successfully changed workflow status". Cập nhật này sẽ dẫn đến việc bạn nhận được một email chứa JSON cho phát hiện này. Bạn sẽ muốn hủy đăng ký khỏi những thông báo này trước khi hoàn thành workshop.
![VPC](/images/5/5.1/s29.png)

Notification:
![VPC](/images/5/5.1/s29b.png)

#### Thách thức
Đến đây bạn đã hiểu hầu hết các nguyên tắc cơ bản về cách **AWS Security Hub** hoạt động với **Amazon EventBridge**. Hãy kiểm tra kiến thức của bạn.

Sau khi cấu hình thông báo cho tất cả các findings có mức độ nghiêm trọng **HIGH** và **CRITICAL**, bạn nhận ra rằng bạn đang nhận được quá nhiều thông báo giữa các thay đổi tuân thủ tài nguyên, giải quyết các phát hiện và đồng nghiệp thay đổi **workflow status** của findings.

Sử dụng những gì bạn đã học cho đến nay, hãy thử sửa đổi thông báo bạn đã thiết lập để chỉ gửi email cho bạn các findings có nhãn mức độ nghiêm trọng **HIGH**, có **workflow status** là **New**, và mà **Security Hub** nhận được từ **GuardDuty** (các findings phải khớp với cả 3 điều kiện). **Amazon GuardDuty** là một dịch vụ phát hiện mối đe dọa liên tục giám sát tài khoản và khối lượng công việc **AWS** của bạn để phát hiện hoạt động độc hại và cung cấp các phát hiện bảo mật chi tiết để bạn có thể nhìn thấy và xử lý chúng.

![VPC](/images/5/5.1/c1.png)
