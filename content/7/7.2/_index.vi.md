---
title : "Quản lý lỗ hổng bảo mật cho các ứng dụng serverless"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Đội ngũ của bạn đang hợp tác với một nhóm phát triển ứng dụng để tạo ra một ứng dụng bảo mật mới bằng cách sử dụng AWS Serverless Application Model. Công việc của bạn là sử dụng Amazon Inspector để phát hiện và khắc phục các lỗ hổng bảo mật trong ứng dụng ngay khi chúng được phát hiện.
{{%/notice%}}

Bạn sẽ triển khai ứng dụng severless với AWS Serverless Application Model. AWS Serverless Application Model (SAM) là một framework mã nguồn mở để xây dựng các ứng dụng severless. Nó cung cấp cú pháp rút gọn để biểu diễn các hàm, API, cơ sở dữ liệu và ánh xạ nguồn sự kiện.

Bạn sẽ bắt đầu bằng cách sử dụng AWS Cloud9 để sao chép ứng dụng từ AWS CodeCommit. AWS Cloud9 là một môi trường phát triển tích hợp trên đám mây (IDE) cho phép bạn viết, chạy và gỡ lỗi code của mình chỉ với một trình duyệt. Nó bao gồm trình chỉnh sửa code, trình gỡ lỗi và terminal. Cloud9 được cài sẵn các công cụ cần thiết cho các ngôn ngữ lập trình phổ biến và AWS Command Line Interface (CLI) đã được cài đặt sẵn nên bạn không cần cài đặt các tệp hoặc cấu hình máy tính của mình cho workshop này.

{{%notice warning%}}
Tại thời workshop thảo này được tóm tắt, Cloud9 và CodeCommit không còn được hỗ trợ. Vì vậy, phần này chỉ để tham khảo.
{{%/notice%}}

#### Thiết lập Cloud9 và triển khai ứng dụng severless của bạn
1.	Truy cập AWS Cloud9, https://us-east-1.console.aws.amazon.com/cloud9control/home?region=us-east-1 


2.	Nhấp vào Create Environment.


3.	Nhập "serverless-appsec" vào Name field và giữ nguyên các tùy chọn mặc định khác.


4.	Nhấp vào Create ở cuối trang.


5.	Đợi thông báo "Successfully created serverless-appsec...".


6.	Nhấp vào liên kết được hiển thị để mở IDE Cloud9.


7.	Bạn sẽ thấy một terminal ở cuối IDE. Bạn có thể mở rộng nó để có nhiều không gian làm việc hơn. Bạn sẽ chạy các lệnh CLI trong terminal này trong suốt workshop. Giữ cho IDE Cloud9 mở trong trình duyệt.


8.	Xác minh rằng người dùng của bạn đã đăng nhập bằng cách chạy lệnh sau trong terminal của Cloud9 ở cuối trang.
```aws sts get-caller-identity```

9. Bạn sẽ nhận được phản hồi bao gồm Account, UserId và Arn của bạn.


10. Chúng tôi đã có một ứng dụng trong AWS CodeCommit. Nếu muốn, bạn có thể dành một chút thời gian để kiểm tra nó. https://us-east-1.console.aws.amazon.com/codesuite/codecommit/repositories/appsec-serverless-demoapp/browse?region=us-east-1 

![VPC](/images/7/7.2/s10.png)

![VPC](/images/7/7.2/s10b.png)
11. Hãy sao chép dụng hiện có vào môi trường Cloud9. Chạy lệnh sau trong terminal của Cloud9.
```git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/appsec-serverless-demoapp```


12. Chuyển đến thư mục ứng dụng bằng cách chạy lệnh sau.
``` cd appsec-serverless-demoapp ```


13. Ứng dụng mẫu được xây dựng bằng AWS SAM. Khi đang ở trong thư mục gốc của ứng dụng, sử dụng lệnh sau để xây dựng ứng dụng và đợi cho đến khi hoàn tất.
```sam build --use-container```


14. Tiếp theo, sử dụng lệnh deploy với tùy chọn guided để chỉ định cấu hình triển khai. Nó sẽ nhắc bạn nhiều lần, như được ghi lại trong hướng dẫn sau.
```sam deploy --guided```
![VPC](/images/7/7.2/s14a.png)
Khi bạn build, AWS SAM CLI sẽ tạo thư mục .aws-sam và sắp xếp các dependency của hàm, code của dự án và các tệp dự án của bạn ở đó.
![VPC](/images/7/7.2/s14r.png)

15.	Tại prompt Stack Name [appsec-serverless-demoapp2]: nhấn enter.


16.	Tại prompt AWS Region [us-east-1]: nhấn enter.


17.	Tại prompt Confirm changes before deploy [Y/n]: nhập "Y" và nhấn enter.


18.	Tại prompt Allow SAM CLI IAM role creation [Y/n]: nhập "Y" và nhấn enter.


19.	Tại prompt Disable rollback [y/N]: nhập "N" và nhấn enter.


20.	Tại prompt HelloWorldFunction has no authentication. Is this okay? [y/N]: nhập "y" và nhấn enter.


21.	Tại prompt LoggingFunction has no authentication. Is this okay? [y/N]: nhập "y" và nhấn enter..


22.	Tại prompt Save arguments to configuration file [Y/n]: nhập "Y" và nhấn enter.


23.	Tại prompt SAM configuration file [samconfig.toml]: nhấn enter.



24.	Tại prompt SAM configuration environment [default]: nhấn enter.
![VPC](/images/7/7.2/s24.png)

25. Tại prompt Deploy this changeset? [y/N]: nhập "y" và nhấn enter.


26. Sẽ mất khoảng 2 phút để tạo stack. Có thể mất vài phút để Amazon Inspector phát hiện các hàm mới và tạo ra các phát hiện.

Bạn có thể kiểm tra lại từ CloudFormation Stacks:

#### Xem lại các phát hiện trong Amazon Inspector
27. Mở Amazon Inspector trong một tab mới. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1 


28. Từ thanh điều hướng, mở trang By Lambda function dưới phần Findings.


29. Ở đây bạn sẽ thấy một số hàm Lambda bị gắn cờ có lỗ hổng bảo mật. Nhấp vào tên của hàm bắt đầu bằng appsec-serverless-demoapp2-LoggingFunction- để xem chi tiết hàm Lambda. Nếu bạn chưa thấy, bạn có thể cần đợi thêm vài phút và làm mới trang.
![VPC](/images/7/7.2/s29.png)

Phát hiện của nó:
![VPC](/images/7/7.2/s29b.png)

Phát hiện CWE tương ứng:
![VPC](/images/7/7.2/s29c.png)
![VPC](/images/7/7.2/s29d.png)
![VPC](/images/7/7.2/s29e.png)
30. Lưu ý có một phát hiện với tiêu đề "CWE-117,93 - Log injection". Nhấp vào tiêu đề của phát hiện để mở rộng chi tiết phát hiện. Ở đây bạn có thể xem thông tin quan trọng, bao gồm mô tả và mức độ nghiêm trọng. Báo cáo lỗ hổng bảo mật cũng bao gồm đường dẫn tệp, vị trí lỗ hổng và đề xuất khắc phục, bao gồm cả đề xuất thay đổi code. Lưu ý thay đổi bạn cần thực hiện.
#### Khắc phục lỗ hổng code
31. Quay lại IDE Cloud9.


32. Tìm thư mục tệp trong bảng điều hướng bên trái. Mở tệp app.py nằm trong thư mục "logging_sample".
![VPC](/images/7/7.2/s32.png)

33. Làm theo đề xuất khắc phục từ Inspector. Thay thế dòng code sau:
```logger.info("Processing %s", filename)```

bằng dòng code này:
```logger.info("Processing %s", urllib.parse.quote(filename))```

Nếu bạn gặp lỗi, có thể do đề xuất yêu cầu một import mới. Nếu bạn gặp lỗi, hãy đảm bảo kiểm tra các import và khoảng cách của bạn. Thực hiện các thay đổi cần thiết. Code của bạn sẽ trông giống như:
![VPC](/images/7/7.2/s33.png)

34. Lưu tệp, sau đó build lại ứng dụng sam bằng lệnh sau.
```sam build --use-container```

![VPC](/images/7/7.2/s34.png)
35. Sau khi quá trình build hoàn tất, hãy deploy ứng dụng bằng lệnh sau và chờ quá trình triển khai hoàn thành.
```sam deploy --guided```



36. Sử dụng các phản hồi tương tự cho việc triển khai được hướng dẫn mà bạn đã đưa ra ở các bước 15-24.


37. Tại prompt Deploy this changeset? [y/N]: nhập "y" và nhấn enter.
![VPC](/images/7/7.2/s37.png)
#### Xác minh việc khắc phục.
38. Quay lại trang By Lambda function trong Amazon Inspector. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/findings/lambda/ 
![VPC](/images/7/7.2/s38.png)

39. Nhấp lại vào tên của hàm bắt đầu bằng appsec-serverless-demoapp2-LoggingFunction- để xem chi tiết hàm Lambda.
![VPC](/images/7/7.2/s39.png)

40. Đặt Finding status bên cạnh Filter criteria thành "Show all".



41. Phát hiện có tiêu đề "CWE-117,93 - Log injection" giờ đây sẽ có trạng thái là Closed. Nếu không, chờ vài phút. Inspector sẽ tự động phát hiện sự thay đổi và cập nhật phát hiện cho phù hợp.

![VPC](/images/7/7.2/s41.png)