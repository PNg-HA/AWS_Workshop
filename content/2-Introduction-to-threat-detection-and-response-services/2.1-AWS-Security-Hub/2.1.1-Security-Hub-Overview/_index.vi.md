---
title : "Security Hub - Tổng quan"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1.1 </b> "
---


#### Tiêu chuẩn bảo mật
1. Điều hướng đến [AWS Security Hub summary page](https://console.aws.amazon.com/securityhub/home?#/summary).

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s1.png) 
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s1b.png)
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s1c.png)  
2. Từ trang Summary bạn có thể xem những tiêu chuẩn nào đã được bật trong tài khoản của mình và bạn có thể thấy số lượng kiểm soát đạt và không đạt trên mỗi tiêu chuẩn. Tỷ lệ phần trăm là phần trăm các biện pháp kiểm soát ở trạng thái tuân thủ "passed" so với số lượng các biện pháp kiểm soát có trạng thái tuân thủ "passed" hoặc "failed".
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s2.png)

3. Chọn **Security standards** trong thanh điều hướng bên trái.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s3.png)

4. Các tiêu chuẩn đã được bật bằng cách sử dụng CloudFormation khi workshop được cung cấp.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s4.png)

5. Hãy đi sâu vào một trong những tiêu chuẩn. Chọn **View Results** để xem NIST Special Publication 800-53 Revision 5.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s5.png)

#### Kiểm soát bảo mật
6. Tại đầu trang, bạn có thể chọn các mục để chỉ hiển thị các kiểm soát "Passed", "Failed", "Disabled", v.v.  Bạn cũng có thể lọc để tìm một kiểm soát bằng cách sử dụng thanh tìm kiếm có ghi  "Filter all enabled controls". Chọn thanh tìm kiếm và lọc theo ID. Chỉ định **ID = EC2.13**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s6.png)

7. Chọn Tiêu đề: **Security groups should not allow ingress from 0.0.0.0/0 to port 22**.  Điều này sẽ hiển thị một danh sách tất cả các tài nguyên đã được đánh giá cho kiểm soát cụ thể này và trạng thái hiện tại của từng tài nguyên liên quan đến kiểm soát. Lưu ý rằng có một số tài nguyên có trạng thái **FAILED** và một số có trạng thái **PASSED** .
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s7.png)

8. Các Tiêu chuẩn Bảo mật của AWS Security Hub cung cấp hướng dẫn khắc phục cho từng kiểm tra. Ở đầu trang, chọn **Remediation instructions link**  để mở hướng dẫn trong một trang mới.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s8.png)

#### Investigating a misconfigured resource

1. Quay lại kiểm soát bảo mật của Security Hub mà bạn đang xem.  Đối với một trong những tài nguyên có trạng thái **FAILED**, mở  **Config rule** trong cột **Investigate**. Điều này sẽ hiển thị các liên kết dẫn bạn đến AWS Config để xem dòng thời gian cấu hình của tài nguyên này hoặc quy tắc cấu hình tổng thể đã thực hiện đánh giá trên tài nguyên này.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s9.png)

2.  Chọn **Configuration timeline**. Thao tác này sẽ mở AWS Config và hiển thị dòng thời gian cấu hình tài nguyên cho security group này. Từ đây, bạn có thể điều tra thời điểm mà tài nguyên trở nên không tuân thủ bằng cách xem xét các sự kiện đã được đánh dấu thời gian. Nếu muốn, bạn có thể dành một phút để mở rộng một vài sự kiện.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s10.png)
3.  Mở rộng **CloudTrail Event** được liệt kê.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s11.png)
4.  Sau đó, chọn **CloudTrail** dưới mục View event. Nó sẽ mở sự kiện đó, được ghi lại trong CloudTrail. AWS CloudTrail iám sát và ghi lại hoạt động tài khoản trên toàn bộ cơ sở hạ tầng AWS của bạn, cho phép bạn kiểm soát các hành động lưu trữ, phân tích và khắc phục.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s12.png)
#### Kiểm soát hợp nhất
1.  Quay lại Security Hub  và mở trang General settings từ bảng điều hướng. https://console.aws.amazon.com/securityhub/home?#/settings/general 
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s13.png)
2.   Ở đầu trang, bạn sẽ thấy rằng cả hai tùy chọn "Auto-enable new controls in enabled standards" và  "Consolidated control findings" đều đã được bật.

3.  Chọn Controls từ bảng điều hướng. Trang này hiển thị tương tự như những gì bạn đã xem ở cấp độ tiêu chuẩn, nhưng ở đây bạn sẽ thấy danh sách đầy đủ các kiểm soát hợp nhất.  Điều này giúp dễ dàng hơn để xem có bao nhiêu kiểm soát cá nhân trong tài khoản của bạn đang vượt qua, thất bại, hoặc ở trạng thái khác. Thêm vào đó, với việc kích hoạt các phát hiện hợp nhất, các kiểm soát được bao gồm trong nhiều tiêu chuẩn đã bật không còn tạo ra phát hiện theo từng tiêu chuẩn (trùng lặp). Điều này giúp quản lý số lượng phát hiện trong môi trường của bạn và loại bỏ sự nhầm lẫn tiềm ẩn xung quanh các phát hiện trùng lặp.

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s15.png)

#### Insights
1. Trong thanh điều hướng bên trái, chọn **Insights**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s16.png)
2.  Trong thanh tìm kiếm, nhập **"severity"** và chọn insight có tên **24. Severity by counts of findings**.
![VPC](/images/2/2.1-AWS-Security-Hub/s17.png)
3.  Security Hub cung cấp một số insight quản lý sẵn có. Bạn không thể sửa đổi hoặc xóa các insight quản lý. **24. Severity by counts of findings** là một trong những insight quản lý sẵn có. Hãy dành một ít thời gian để xem số lượng phát hiện trong môi trường. Chọn một **Severity Label** (ví dụ: Critical) để xem các phát hiện cơ bản.
![VPC](/images/2/2.1-AWS-Security-Hub/s18.png)