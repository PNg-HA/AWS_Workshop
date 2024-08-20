---
title : "Tự động ứng phó bảo mật trên AWS"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---
{{%notice info%}}
**Scenario / Problem Statement**: Ở quy mô của công ty bạn, bạn nhận ra rằng có hàng ngàn tài nguyên bị cấu hình sai. Bạn cần tìm giải pháp để khắc phục các cấu hình sai một cách tự động, tập trung và ở quy mô lớn.
{{%/notice%}}

{{%notice info%}}
Trong workshop này, chỉ sử dụng một tài khoản nên giải pháp này không hiển thị tất cả các tính năng của nó. Để xem toàn bộ kịch bản của giải pháp này, vui lòng xem triển khai của tôi tại [GitHub repo này](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub).
{{%/notice%}}


![S3](/images/5/5.3/automated-security-response-on-aws.png)


#### Deploy Automated Security Response on AWS via CloudFormation
{{%notice warning%}}
Giải pháp **ASR** yêu cầu triển khai 3 **CloudFormation templates**. Bạn có thể xem các template [tại đây](https://docs.aws.amazon.com/solutions/latest/automated-security-response-on-aws/solution-overview.html). Bạn có thể khởi chạy tất cả các stack cùng một lúc. Bạn không cần phải chờ một stack hoàn thành trước khi bắt đầu stack tiếp theo. Ba stack này sẽ mất 10 phút để hoàn thành. Các stack cũng sẽ khởi chạy nhiều **nested stacks** để tạo tất cả các tài nguyên cần thiết.
{{%/notice%}}

1. Triển khai **admin stack** bằng cách nhấp vào nút **Deploy Automated Security Response admin stack** bên dưới. Đảm bảo rằng bạn đang khởi chạy template trong vùng mà bạn đã làm việc. Để khởi chạy giải pháp này trong một **AWS Region** khác, sử dụng **Region selector** trong thanh điều hướng của **AWS Management Console**.
[Deploy Automated Security Response admin stack admin stack](https://console.aws.amazon.com/cloudformation/home?#/stacks/create/review?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Faws-security-hub-automated-response-and-remediation%2Flatest%2Faws-sharr-deploy.template&stackName=aws-security-hub-automated-response-and-remediation-admin&param_LoadAFSBPAdminStack=no&param_LoadCIS120AdminStack=no&param_LoadCIS140AdminStack=no&param_LoadNIST80053AdminStack=no&param_LoadPCI321AdminStack=no&param_LoadSCAdminStack=yes&ReuseOrchestratorLogGroup=no)

2. Xem lại tên stack và các tham số cho template. Các tham số đã được điền sẵn, nhưng hãy xác nhận rằng các tham số được chọn khớp với ảnh chụp màn hình dưới đây. Cuộn xuống cuối màn hình **Quick create stack** và đánh dấu vào ô **I acknowledge that AWS CloudFormation might create IAM resources** và ô **I acknowledge that AWS CloudFormation might require the following capability**: **CAPABILITY_AUTO_EXPAND**.

![VPC](/images/5/5.3/s2.png)
![VPC](/images/5/5.3/s2b.png)

3. Nhấp vào **Create stack**. Bạn có thể tiếp tục mà không cần đợi stack hoàn thành.

4. Triển khai **member roles stack** bằng cách nhấp vào nút **Deploy ASR member roles stack** bên dưới. Đảm bảo rằng bạn đang khởi chạy template trong vùng mà bạn đã làm việc. Để khởi chạy giải pháp này trong một **AWS Region** khác, sử dụng **Region selector** trong thanh điều hướng của **AWS Management Console**.
 [Deploy ASR member roles stack](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Faws-security-hub-automated-response-and-remediation%2Flatest%2Faws-sharr-member.template&stackName=aws-security-hub-automated-response-and-remediation-member&param_LogGroupName=SHARR-Log-Group&param_LoadAFSBPMemberStack=no&param_LoadCIS120MemberStack=no&param_LoadCIS140MemberStack=no&param_LoadNIST80053MemberStack=no&param_LoadPCI321MemberStack=no&param_LoadSCMemberStack=yes)

5. Xem lại tên stack và các tham số cho template. Đối với tham số **SecHubAdminAccount**, nhập **account ID** mà bạn thấy ở góc trên bên phải của **AWS Console**.

6. Cuộn xuống cuối màn hình **Quick create stack** và đánh dấu vào ô **I acknowledge that AWS CloudFormation might create IAM resources**.

7. Nhấp vào **Create stack**.

8. Triển khai **member runbooks stack** bằng cách nhấp vào nút **Deploy ASR member runbooks stack** bên dưới. Đảm bảo rằng bạn đang khởi chạy template trong vùng mà bạn đã làm việc. Để khởi chạy giải pháp này trong một **AWS Region** khác, sử dụng **Region selector** trong thanh điều hướng của **AWS Management Console**.

Deploy ASR member runbooks stack: [Deploy ASR member runbooks stack](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Faws-security-hub-automated-response-and-remediation%2Flatest%2Faws-sharr-member.template&stackName=aws-security-hub-automated-response-and-remediation-member&param_LogGroupName=SHARR-Log-Group&param_LoadAFSBPMemberStack=no&param_LoadCIS120MemberStack=no&param_LoadCIS140MemberStack=no&param_LoadNIST80053MemberStack=no&param_LoadPCI321MemberStack=no&param_LoadSCMemberStack=yes).


9. Xem lại tên stack và các tham số cho template. Các tham số đã được điền sẵn, nhưng hãy xác nhận rằng các tham số được chọn khớp với ảnh chụp màn hình dưới đây. Đối với tham số **SecHubAdminAccount**, nhập lại **account ID** mà bạn thấy ở góc trên bên phải của **AWS Console**.
![VPC](/images/5/5.3/s9.png)

10. Cuộn xuống cuối màn hình **Quick create stack** và đánh dấu vào ô **I acknowledge that AWS CloudFormation might create IAM resources** và ô **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**.

11. Nhấp vào **Create stack**.

12. Tại thời điểm này, bạn phải đợi tất cả các **CloudFormation stacks** hoàn thành triển khai. Bạn có thể kiểm tra tiến độ tại đây: https://console.aws.amazon.com/cloudformation/home?#/stacks 
![VPC](/images/5/5.3/s12.png)

#### Leverage Automated Security Response on AWS for remediation
Giải pháp này bao gồm các **playbook remediations** cho các tiêu chuẩn bảo mật được định nghĩa trong **Center for Internet Security (CIS) AWS Foundations Benchmark v1.2.0**, **Center for Internet Security (CIS) AWS Foundations Benchmark v1.4.0**, **AWS Foundation Security Best Practices (AFSBP) v.1.0.0**, **Payment Card Industry Data Security Standard (PCI-DSS) v3.2.1**, và **Security Control (SC) v2.0.0**. Bạn có thể tìm hiểu thêm tại đây: https://docs.aws.amazon.com/solutions/latest/automated-security-response-on-aws/playbooks-1.html 
13. Hãy thử giải pháp này. Quay lại [Security Hub](https://console.aws.amazon.com/securityhub).

14. Mở trang **Controls**.

15. Lọc theo **control EC2.2**.
![VPC](/images/5/5.3/s15.png)

16. Nhấp vào tên **The VPC default security group should not allow inbound and outbound traffic**.
![VPC](/images/5/5.3/s16.png)

17. Chọn hộp kiểm để chọn tất cả các kiểm tra thất bại.

18. Nhấp vào menu thả xuống **Actions** và chọn **Remediate with ASR**. Việc chọn hành động này sẽ gửi một bản sao của phát hiện(s) đến **EventBridge**, khởi động tự động hóa để thay đổi cài đặt quy tắc **security group** mặc định để hạn chế lưu lượng inbound và outbound.

19. Sau vài phút, các kiểm tra sẽ có trạng thái là **RESOLVED**.

{{%notice warning%}}
Có thể mất khoảng 3 đến 5 phút để chạy và cập nhật. Sau đó, đợi và làm mới trang lại.
{{%/notice%}}

### Deep dive to the solution
#### Terminology
- **AWS Config rule**: một cấu hình lý tưởng
- **Security control**: đại diện của một rule -> trong một hoặc nhiều tiêu chuẩn bảo mật
- **Finding**: một vấn đề bảo mật tiềm ẩn được tạo ra sau khi kiểm tra bảo mật
- **Playbook**: một tập hợp các biện pháp khắc phục.
- **Remediation runbook**: một triển khai của một tập hợp các bước để giải quyết một finding.
- **Control runbook**: các tài liệu **SSM automation** mà **Orchestrator** sử dụng để định tuyến một biện pháp khắc phục đã được kích hoạt cho một control cụ thể đến **remediation runbook** phù hợp.
#### Workflow of the architecture
Với **AWS Config rules** cho các tiêu chuẩn tuân thủ được hỗ trợ, **AWS Config** thực thi kiểm tra **security controls** và tạo **findings** nếu có các cấu hình không tuân thủ, bao gồm trong tài khoản quản trị và thành viên (trong **AWS Organization**). Các **findings** được tổng hợp bởi **AWS Security Hub**. Quản trị viên có thể kích hoạt **Custom Action** để khắc phục các **findings** tương ứng tự động hoặc thủ công. Các sự kiện này được kích hoạt trong các **EventBridge rules** tương ứng đến **Step Function Orchestrator**, điều hướng đến **playbooks remediation** tương ứng (ví dụ: findings cho **EC2.13** dẫn đến **EC2.13 playbook**). Dịch vụ **Amazon SQS** được sử dụng để thực thi nhiều biện pháp khắc phục song song. **Orchestrator** sẽ thông báo cho người dùng đã đăng ký về quy trình và kết quả khắc phục. Nó sẽ gọi **control runbook** tương ứng, cuối cùng sẽ gọi **remediation runbook** thích hợp cho các findings.

#### An example of security control
Trong phần dive-deep này, tôi sử dụng **security control EC2.13** để kiểm tra cấu hình tài nguyên và khắc phục nếu không tuân thủ. Có các ví dụ khắc phục khác bên dưới, nhưng luồng khắc phục tương tự như **EC2.13**.
![VPC](/images/5/5.3/d1.png)

#### Control Runbook EC2.13
![VPC](/images/5/5.3/d2.png)
##### First step
Bước đầu tiên của **Control Runbook** là phân tích các đầu vào nhận được từ **finding**:
![VPC](/images/5/5.3/d_step1.png)
Phiên bản **yaml** của **runbook**:

```
expected_control_id:
  - EC2.13
  - EC2.14
parse_id_pattern: ^arn:(?:aws|aws-cn|aws-us-gov):ec2:(?:[a-z]{2}(?:-gov)?-[a-z]+-\d):\d{12}:security-group\/(sg-[a-f\d]{8,17})$
Finding: '{{ Finding }}'
```
And the expected output of this first step is:
![VPC](/images/5/5.3/d3.png)

##### Second step
Trong bước 2, **control runbook** gọi **remediation runbook (SSM automation document)** có tên là **AWS-DisablePublicAccessForSecurityGroup**. Bạn có thể tham khảo thêm tại [AWS Systems Manager Automation runbook reference](https://docs.aws.amazon.com/systems-manager-automation-runbooks/latest/userguide/automation-aws-disablepublicaccessforsecuritygroup.html), và kiểm tra luồng logic tại [System Manager page - Document section](https://ap-southeast-1.console.aws.amazon.com/systems-manager/documents/AWS-DisablePublicAccessForSecurityGroup/description?region=ap-southeast-1#).

![VPC](/images/5/5.3/d_step2.png)
Phiên bản **yaml** của bước này:
![VPC](/images/5/5.3/d_step2b.png)

**Remediation runbook** sẽ kích hoạt **Amazon EC2 API** để loại bỏ các **inbound rules** đáp ứng tham số của bạn trong cuộc gọi API. Trong tài liệu tự động này, nó thu hồi bất kỳ **inbound rules** nào với giao thức **TCP port 22 (SSH)** và phạm vi **IP** là **0.0.0.0/0**:
![VPC](/images/5/5.3/d_step2c.png)

#### Demo
##### Prerequisite
![Prerequisite](/images/5/5.3/d4.png)

##### Behind the scene: Orchestrator
![Orchestrator](/images/5/5.3/d5.png)

##### Scenarios
Link demo: https://www.youtube.com/playlist?list=PL7IdJecfX87jHfO43NYd6MXL8mBYWBAIf
1.1. Các Security groups không nên cho phép ingress từ 0.0.0.0/0 đến port 22 - trong member \
1.2. Các Security groups không nên cho phép ingress từ 0.0.0.0/0 đến port 22 - trong admin \
2. Đảm bảo chính sách mật khẩu IAM yêu cầu ít nhất một chữ số \
3. RDS DB clusters nên được cấu hình cho nhiều AZs \
4. Mã hóa mặc định của EBS nên được kích hoạt \
5. Các S3 general purpose buckets nên bật cài đặt chặn truy cập công khai
