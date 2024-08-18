---
title : "Tập hợp các phát hiện từ các giải pháp của đối tác AWS"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3. </b> "
---
AWS Security Hub có thể tổng hợp dữ liệu phát hiện bảo mật từ nhiều dịch vụ AWS và từ các giải pháp bảo mật của AWS Partner Network (APN) được hỗ trợ. Việc tổng hợp này cung cấp cái nhìn tổng quát về bảo mật và tuân thủ trong toàn bộ môi trường AWS của bạn.

Bạn cũng có thể gửi các phát hiện được tạo ra từ các sản phẩm bảo mật tùy chỉnh của riêng bạn.

Từ các tích hợp sản phẩm AWS và đối tác được hỗ trợ, Security Hub chỉ nhận và hợp nhất các phát hiện được tạo ra sau khi bạn bật Security Hub trong các tài khoản AWS của bạn. Dịch vụ này không nhận và hợp nhất các phát hiện bảo mật được tạo ra trước khi bạn bật Security Hub.\

Một trong các tích hợp đối tác có sẵn là Cloud Custodian. Cloud Custodian cho phép bạn quản lý các tài nguyên đám mây của mình bằng cách lọc, gán thẻ và sau đó thực hiện các hành động trên chúng. YAML DSL cho phép định nghĩa các quy tắc để có cơ sở hạ tầng đám mây được quản lý tốt, bảo mật và tối ưu hóa chi phí. Để biết thêm thông tin, hãy truy cập https://cloudcustodian.io/ 

#### Nhận các phát hiện từ Cloud Custodian
1. Mở trang **Integrations** trong Security Hub và tìm kiếm **Cloud Custodian**.
![VPC](/images/3/3.3/s1.png)

2. Chọn **Accept Findings**. Xem xét các quyền cần thiết cho tích hợp.

![VPC](/images/3/3.3/s2.png)
3. Chọn **Accept findings**. Điều này sẽ thiết lập một chính sách dịch vụ cho phép giải pháp đối tác gửi thông tin phát hiện vào tài khoản này. Đối với mục đích của workshop này, một phiên bản Cloud Custodian đã được thiết lập để tự động gửi các phát hiện đến tích hợp bạn vừa bật. Để sử dụng các tích hợp đối tác khác trong tài khoản của bạn, bạn vẫn cần hoàn thành bước cấu hình trong giải pháp đối tác để các phát hiện được tạo ra bởi giải pháp đối tác được gửi đến Security Hub.


Bạn có thể chọn See findings để xem các phát hiện mới từ Cloud Custodian. Có thể mất 5-10 phút để chúng xuất hiện.