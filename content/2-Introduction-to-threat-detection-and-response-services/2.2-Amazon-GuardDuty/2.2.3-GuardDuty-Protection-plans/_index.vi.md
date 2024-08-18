---
title : "GuardDuty - Kế hoạch bảo vệ"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2.3 </b> "
---
#### Điều kiện tiên quyết
Đảm bảo rằng EC2 instance có SSM Management Role. Để biết cách thêm instance profile vào EC2 instance, vui lòng tham khảo thêm tại https://docs.aws.amazon.com/systems-manager/latest/userguide/setup-instance-permissions.html#attach-instance-profile.
#### S3 Protection trong Amazon GuardDuty

1. Truy cập trang **S3 Protection** dưới phần Protection plans trong bảng điều khiển GuardDuty.


2. Đảm bảo rằng S3 Protection đã được bật.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s2.png)

#### EKS Protection trong Amazon GuardDuty
3. Truy cập trang **EKS Protection** dưới phần Protection plans trong bảng điều khiển GuardDuty.


4. Đảm bảo rằng EKS Audit Log Monitoring đã được bật. Lưu ý: Trải nghiệm trên bảng điều khiển cho GuardDuty EKS Runtime Monitoring hiện được quản lý như một phần của tính năng Runtime Monitoring mới.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s4.png)
#### Runtime Monitoring trong Amazon GuardDuty

5. Truy cập trang Runtime Monitoring dưới phần Protection plans trong bảng điều khiển GuardDuty.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s5.png)

1. Đảm bảo rằng Runtime Monitoring đã được bật cùng với cấu hình Automated agent configuration cho từng Amazon EKS, AWS Fargate (chỉ ECS), và  Amazon EC2.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s6.png)


1. Trong trang Runtime Monitoring, chuyển sang tab Runtime coverage. "Coverage statistics" là gì? Mặc dù không nằm trong phạm vi của workshop này, tìm hiểu thêm về cách thiết lập Runtime Monitoring tại https://docs.aws.amazon.com/guardduty/latest/ug/runtime-monitoring.html .
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s7a.png)

*Lưu ý*: Nếu **Coverage status** của EC2 Instance là unhealthy và trạng thái **Issue** là "**No Agent Reporting**" hoặc liên quan đến SSM
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/errorSSM.png)
có thể là do không thể cài đặt SSM agent trong EC2 instance. Bạn có thể làm rõ bằng cách kiểm tra /var/log/amzn-guardduty-agent trong instance (AL2, AL2023). Đọc thêm tại https://docs.aws.amazon.com/guardduty/latest/ug/gdu-assess-coverage-ec2.html#ec2-runtime-monitoring-coverage-issues-troubleshoot

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s7b.png)

#### Malware Protection cho EC2 trong Amazon GuardDuty

8. Truy cập trang **Malware Protection** dưới mục Protection plans trong bảng điều khiển GuardDuty.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s8.png)

9. GuardDuty tự động khởi động quét malware sau khi phát hiện một phát hiện chỉ ra malware trong một EC2 instance hoặc một khối lượng công việc container. Đảm bảo rằng GuardDuty-initiated malware scan đã được bật.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s9.png)
Dưới đây là một số EC2 malware scans
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s9b.png)
Nếu bạn theo dõi scan qua Scan ID
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s9c.png)
Tệp EICAR-Test là một tệp để kiểm tra xem chức năng phát hiện mối đe dọa có hoạt động hay không. Nó không phải là virus thực sự.

10. Cuộn xuống và bật **Retain scanned snapshots when malware is detected** on.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s10.png)
#### RDS Protection trong Amazon GuardDuty

11. Truy cập trang **RDS Protection** dưới mục Protection plans trong bảng điều khiển GuardDuty.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s11.png)

12. Đảm bảo rằng RDS Login Activity Monitoring đã được bật.


![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s12.png)
#### Lambda Protection trong Amazon GuardDuty
13. Truy cập trang **Lambda Protection** dưới mục Protection plans trong bảng điều khiển GuardDuty.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s13.png)
14. Đảm bảo rằng Lambda Network Activity Monitoring đã được bật.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s14.png)