---
title : "Inspector - Tổng quan"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.3.1 </b> "
---


#### Inspector
Amazon Inspector là dịch vụ quản lý lỗ hổng bảo mật liên tục quét các khối lượng công việc AWS để phát hiện lỗ hổng phần mềm và sự tiếp xúc mạng không mong muốn. Chỉ với vài bước trong AWS Management Console, bạn có thể sử dụng Amazon Inspector trên tất cả các tài khoản trong tổ chức của bạn. Khi được khởi động, nó tự động phát hiện các phiên bản Amazon Elastic Compute Cloud (EC2) instance, container images nằm trong Amazon Elastic Container Registry (ECR) and within continuous integration and continuous delivery (CI/CD) tools, and AWS Lambda functions, at scale, and immediately starts assessing them for known vulnerabilities.

Amazon Inspector tính toán điểm rủi ro rất cụ thể cho từng phát hiện bằng cách liên kết thông tin về CVE với các yếu tố như quyền truy cập mạng và khả năng khai thác. Điểm số này được sử dụng để ưu tiên các lỗ hổng quan trọng nhất nhằm cải thiện hiệu quả phản ứng khắc phục. Tất cả các phát hiện được tập hợp trong bảng điều khiển Amazon Inspector và gửi đến AWS Security Hub và Amazon EventBridge để tự động hóa các quy trình làm việc. Các lỗ hổng tìm thấy trong container image cũng được gửi đến Amazon ECR để các chủ sở hữu tài nguyên có thể xem và khắc phục. Amazon Inspector giúp các đội bảo mật và các nhà phát triển ở bất kỳ quy mô nào đạt được bảo mật và tuân thủ cơ sở hạ tầng toàn diện trong các môi trường AWS của họ.

![VPC](/images/2/2.3/2.3.1/s1.png)