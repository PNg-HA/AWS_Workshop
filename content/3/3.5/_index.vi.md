---
title : "Xây dựng riêng giải pháp tích hợp Security Hub"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5. </b> "
---
Trong mô-đun này, bạn sẽ xây dựng một giải pháp tùy chỉnh để gửi các phát hiện từ Security Hub đến một kênh Slack. Để thực hiện điều này, bạn sẽ thiết lập một hành động tùy chỉnh để gửi chi tiết của một phát hiện từ Security Hub đến một hàm Lambda (qua EventBridge) mà sẽ định dạng tin nhắn và sau đó gửi vào kênh cảnh báo bảo mật Slack. Khi thiết lập hoàn tất, các phát hiện sẽ chảy từ trái sang phải trong sơ đồ dưới đây.

#### Thiết lập không gian làm việc Slack
1. Để phục vụ cho workshop này, chúng tôi khuyến nghị bạn thiết lập một không gian làm việc Slack tạm thời mới. Sử dụng liên kết sau để tạo một không gian làm việc Slack mới: https://get.slack.help/hc/en-us/articles/206845317-Create-a-Slack-workspace 

![VPC](/images/3/3.5/s1.png)

![VPC](/images/3/3.5/s1a.png)
2. Khi bạn đã hoàn tất việc tạo không gian làm việc Slack, hãy tạo một kênh mới có tên là "security-hub-alerts". Đây là nơi chúng tôi sẽ gửi thông báo từ Security Hub bằng cách sử dụng tích hợp tùy chỉnh. Trong ví dụ này, hãy đặt tính khả dụng của kênh thành public.
![VPC](/images/3/3.5/s2a.png)
![VPC](/images/3/3.5/s2b.png)

#### Thiết lập ứng dụng Slack cho tích hợp
3. Truy cập trang Slack API, https://api.slack.com.

![VPC](/images/3/3.5/s3.png)
4. Ở đầu trang, chọn Your apps. Sau đó chọn Create an App.

![VPC](/images/3/3.5/s4.png)
5. Trong cửa sổ Create an app, chọn From scratch.

![VPC](/images/3/3.5/s5.png)

6. Nhập tên ứng dụng là "security-hub-to-slack". Đối với Pick a workspace to develop your app in, chọn không gian làm việc bạn vừa tạo.


7. Chọn **Create App**.
![VPC](/images/3/3.5/s7.png)

8. Mở **Incoming Webhooks**.
![VPC](/images/3/3.5/s8.png)

9.  Trên trang **Activate Incoming Webhooks** bật công tắc lên ON.


10. Sau đó ở cuối trang, chọn **Add New Webhook to Workspace**.
![VPC](/images/3/3.5/s10.png)

11. Trên trang hỏi "Where should security-hub-alerts post?" chọn **#security-hub-alerts**. Chọn **Allow**.


12. Quay lại trang Incoming Webhooks, sao chép **Webhook URL** bạn vừa tạo. Bạn sẽ cần cái này sau.

![VPC](/images/3/3.5/s12.png)
#### Tạo Custom Action trong Security Hub
Slack đã được thiết lập. Bây giờ bạn cần cấu hình Security Hub custom action, EventBridge rule, và Lambda function để đẩy các phát hiện đến ứng dụng Slack của bạn (qua webhook).

13. Trở lại Security Hub. https://console.aws.amazon.com/securityhub/ 


14. Mở trang Custom actions từ thanh điều hướng bên trái dưới Management.


15. Chọn Create custom action.

![VPC](/images/3/3.5/s15.png)
16. Đối với Action name, nhập "Send to Slack". Đối với Description, nhập "Custom action to send findings to Slack." Đối với Custom action ID, nhập "SendToSlack".


17. Sao chép Custom action ARN mà bạn vừa tạo cho Send to Slack. Bạn sẽ cần cái này sau.
#### Tạo the EventBridge Rule

![VPC](/images/3/3.5/s17.png)
Kết quả:
![VPC](/images/3/3.5/s17b.png)
18. Điều hướng đến **Amazon EventBridge**.



19. Chọn **Create rule** ở bên phải.


20. Trong trang **Define rule detail** đặt **tên** cho rule của bạn và **mô tả** nó để thể hiện mục đích của rule (ví dụ, tên và mô tả giống như bạn đã dùng cho hành động tùy chỉnh). Sau đó chọn **Next**.


21. Tất cả các phát hiện của Security Hub được gửi dưới dạng sự kiện đến bus sự kiện mặc định của AWS. Phần định nghĩa mẫu cho phép bạn xác định các bộ lọc để thực hiện hành động cụ thể khi các sự kiện phù hợp xuất hiện. Trong bước **Build event pattern** giữ  **Event source** được đặt là **AWS events or EventBridge partner events**.

22. Cuộn xuống phần Event pattern. Dưới Event source, giữ nó được đặt là AWS Services, và dưới AWS Service, chọn Security Hub.


23. Đối với Event Type, chọn Security Hub Findings – Custom Action.



24. Sau đó chọn nút radio Specific custom action ARN(s) và nhập ARN cho hành động tùy chỉnh mà bạn vừa tạo.



25. Chọn Next.

![VPC](/images/3/3.5/s25.png)

26. Trong bước Select target(s), chọn Lambda function từ danh sách Select a target. Sau đó chọn security-hub-to-slack từ danh sách Function.

![VPC](/images/3/3.5/s26.png)

27. Lambda function, security-hub-to-slack, đã được tạo bằng CloudFormation trước khi bắt đầu lab. Nếu bạn muốn xem code, bạn có thể thấy nó trong bảng điều khiển Lambda. Chọn Next.

![VPC](/images/3/3.5/s27.png)
28. Trong bước Configure tags, chọn Next.

![VPC](/images/3/3.5/s28.png)

29. Trong bước Review and create, chọn Create rule.



#### Tùy chỉnh Lambda function để gửi tin nhắn đến kênh Slack của bạn


30. Điều hướng đến bảng điều khiển Lambda và mở trang Functions.


![VPC](/images/3/3.5/s30.png)

1.  Mở security-hub-to-slack function bằng cách chọn tên của nó.




32. Trong trang function, mở tab Configuration và sau đó chọn Environment variables.

![VPC](/images/3/3.5/s32.png)


1.  Chọn nút **Edit**. Đặt slackChannel là "security-hub-alerts". Đặt webHookUrl thành webhook bạn đã sao chép trước đó trong mô-đun này.

![VPC](/images/3/3.5/s33.png)



34. Chọn Save.


#### Kiểm tra Tích hợp




Tại thời điểm này, mọi thứ đã được thiết lập. Hãy kiểm tra tích hợp!


35.  Truy cập Security Hub và mở trang Findings. https://console.aws.amazon.com/securityhub 


36. Đánh dấu vào ô bên cạnh một phát hiện.

![VPC](/images/3/3.5/s36.png)

37. Dưới menu Actions, chọn Send to Slack.

![VPC](/images/3/3.5/s37.png)
Kết quả:
![VPC](/images/3/3.5/s37b.png)

38. Quay lại Slack và mở kênh bạn đã tạo, #security-hub-alerts. Bạn nên thấy một tin nhắn mới.

Xem kênh “security-hub-alerts” có tin tức

![VPC](/images/3/3.5/s38.png)
Chọn kênh đó để xem:
![VPC](/images/3/3.5/s38b.png)

#### Tự động hóa tin nhắn Slack

**Vấn đề:**: 
Quản lý của bạn lo ngại về khả năng mở rộng của phương pháp bạn thực hiện vì nó yêu cầu bạn phải chọn thủ công các phát hiện để gửi đến Slack. Bạn đã được chỉ đạo tự động hóa điều này để các phát hiện có nhãn mức độ MEDIUM hoặc HIGH được tự động gửi đến Slack thông qua tích hợp.
Bạn có thể tìm ra những thay đổi nào cần thực hiện để đạt được mục tiêu này không?

**Gợi ý**: \
6. Chọn **Specific Severity label(s)** và trong menu thả xuống, chọn **MEDIUM** và **HIGH**. Tiếp tục thực hiện các bước như trước.
![VPC](/images/3/3.5/h6.png)

7. Chọn **Next**.

8. Trong bước Select target(s), chọn Lambda function từ danh sách Select a target. Sau đó chọn security-hub-to-slack từ danh sách Function. Chọn Next.

![VPC](/images/3/3.5/h6-s8.png)

9. Trong bước Configure tags, chọn Next.


10. Trong bước Review and create, chọn Create rule.
![VPC](/images/3/3.5/h6-s10a.png)
![VPC](/images/3/3.5/h6-s10b.png)