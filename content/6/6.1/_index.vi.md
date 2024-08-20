---
title : "Đối phó đối với việc rò rỉ thông tin xác thực IAM Role"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 6.1 </b> "
---
Trong mô-đun này, bạn sẽ thực hiện các bước thủ công để mô phỏng một tình huống (rò rỉ thông tin xác thực IAM Role) mà sau đó bạn sẽ điều tra và khắc phục. Bạn sẽ sao chép thông tin xác thực tạm thời của IAM từ EC2 instance và sử dụng chúng để thực hiện các lệnh API từ máy tính cá nhân của bạn. Sau đó, bạn sẽ điều tra phát hiện của GuardDuty cho hoạt động này.

#### Lấy thông tin xác thực tạm thời của IAM bằng cách sử dụng AWS Systems Manager
Để mô phỏng mối đe dọa này, bạn sẽ lấy thông tin xác thực tạm thời của IAM được tạo bởi IAM Role cho EC2. Bạn có thể SSH trực tiếp vào instance và truy vấn metadata hoặc làm theo các bước dưới đây bằng AWS Systems Manager Session Manager.

Session Manager là một dịch vụ quản lý cung cấp cho bạn quyền truy cập an toàn vào các instance của mình chỉ với một cú nhấp chuột mà không cần mở các cổng inbound và quản lý các máy chủ bastion. Bạn có thể kiểm soát truy cập tập trung vào các instance của mình và có đầy đủ khả năng kiểm tra để đảm bảo tuân thủ các chính sách của công ty.

1. Truy cập [Session Manager](https://us-east-1.console.aws.amazon.com/systems-manager/session-manager?region=us-east-1) trong bảng điều khiển AWS Systems Manager.


2. Nhấp vào nút Start Session.



3. Chọn GuardDuty-ICE-Instance và nhấp vào Start session. Điều này sẽ bắt đầu một phiên shell mà bạn sẽ sử dụng để truy xuất các thông tin xác thực đang hoạt động trên instance.
![VPC](/images/6/6.1/s3.png)

4. Trong shell, chạy lệnh sau:

```
TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/
```
![VPC](/images/6/6.1/s4.png)
Sau đó chạy lệnh này:
```
curl -H "X-aws-ec2-metadata-token: $TOKEN" http://169.254.169.254/latest/meta-data/iam/security-credentials/GDWorkshop-EC2-Compromised
```
![VPC](/images/6/6.1/s4b.png)
5. Ghi lại AccessKeyID, SecretAccessKey, và Token. Bây giờ bạn đã lấy được thông tin xác thực tạm thời của IAM từ EC2 instance, hãy sử dụng chúng để thiết lập một hồ sơ AWS CLI trên máy tính cá nhân của bạn. Sau đó, chúng ta sẽ sử dụng hồ sơ này để thực hiện các lệnh API.


#### Sử dụng thông tin xác thực từ máy tính cá nhân của bạn

{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Trong các bước sau, bạn sẽ mô phỏng việc rò rỉ thông tin xác thực. Để mô phỏng thành công, các lệnh API cần phải được thực hiện từ bên ngoài mạng AWS, nếu không sẽ không tạo ra các phát hiện. Bạn phải chạy các lệnh sau từ máy tính cá nhân của mình. Đừng chạy các lệnh từ AWS CloudShell, từ một tài nguyên AWS khác hoặc khi đang sử dụng VPN của AWS.
{{%/notice%}}

{{%notice tip%}}
Bạn sẽ cần cài đặt AWS CLI trên máy tính cá nhân của mình cho các bước tiếp theo. Nếu bạn chưa cài đặt AWS CLI trên máy tính cá nhân của mình, hãy xem tài liệu hướng dẫn cài đặt: https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html 
{{%/notice%}}


6. Từ command prompt, chạy các lệnh sau (thay thế `<AccessKeyId>`, `<SecretAccessKey>`, và `<Token>` bằng thông tin xác thực của bạn). Đảm bảo rằng region tương ứng với region mà bạn đã làm việc.

```
aws configure set profile.badbob.region us-east-1
```

```
aws configure set profile.badbob.aws_access_key_id <AccessKeyId>
```

```
aws configure set profile.badbob.aws_secret_access_key <SecretAccessKey>
```

```
aws configure set profile.badbob.aws_session_token <Token>
```

7. Bây giờ bạn đã có hồ sơ của mình, bạn có thể sử dụng nó để thực hiện các lệnh API. Khám phá những gì bạn có quyền truy cập bằng cách chạy các lệnh sau để truy vấn các dịch vụ khác nhau. Đừng ngạc nhiên nếu bạn nhận được một số phản hồi bị từ chối truy cập, điều này là có ý đồ. Chạy các lệnh sau. Bạn có quyền truy cập vào IAM không?

```
aws iam get-user --profile badbob
```

Trả lời: Lệnh này sẽ trả về một lỗi AccessDenied.

```
aws iam create-user --user-name Chuck --profile badbob
```
![VPC](/images/6/6.1/s7.png)
8. Còn về DynamoDB thì sao? Hãy thử các lệnh sau.

```aws dynamodb list-tables --profile badbob```

```aws dynamodb describe-table --table-name GuardDuty-Example-Customer-DB --profile badbob```
![VPC](/images/6/6.1/s8.png)

{{%notice warning%}}
Role và hồ sơ instance được sử dụng ở đây không tuân thủ các nguyên tắc đặc quyền tối thiểu. Tìm hiểu thêm tại đây: https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege 
{{%/notice%}}

9. Bạn có thể truy vấn dữ liệu không?
```aws dynamodb scan --table-name GuardDuty-Example-Customer-DB --profile badbob```

Tiếp tục chạy các lệnh khác:

```aws dynamodb scan --table-name GuardDuty-Example-Customer-DB --profile badbob```

```aws dynamodb delete-table --table-name GuardDuty-Example-Customer-DB --profile badbob```
![VPC](/images/6/6.1/s9.png)

Kiểm tra tài nguyên sau khi xóa
```aws dynamodb list-tables --profile badbob```

![VPC](/images/6/6.1/s9b.png)

10.  Bạn có quyền truy cập vào Systems Manager Parameter Store không? Trả lời: Không.

```aws ssm describe-parameters --profile badbob```

```aws ssm get-parameters --names "gd_prod_dbpwd_sample" --profile badbob```

```aws ssm get-parameters --names "gd_prod_dbpwd_sample" --with-decryption --profile badbob```

```aws ssm delete-parameter --name "gd_prod_dbpwd_sample" --profile badbob```
![VPC](/images/6/6.1/s10.png)
### Điều tra phát hiện của GuardDuty
{{%notice warning%}}
Có thể mất đến 30 phút để thấy được phát hiện từ các hành động này trong bảng điều khiển GuardDuty. Chúng tôi khuyên bạn nên làm việc với mô-đun tiếp theo và quay lại bước này sau.
{{%/notice%}}

{{%notice info%}}
Tình huống / Báo cáo vấn đề
Bạn đã được chỉ định một phát hiện của GuardDuty để điều tra và khắc phục. Loại phát hiện của GuardDuty là "UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS".
{{%/notice%}}


11. Trong các bước trên, bạn đã mô phỏng việc rò rỉ thông tin xác thực IAM Role. Bây giờ bạn sẽ điều tra phát hiện của GuardDuty được tạo ra từ các hành động bạn đã thực hiện. Bắt đầu bằng cách điều hướng đến bảng điều khiển GuardDuty.


12. Bạn sẽ thấy một phát hiện với loại UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS. Nếu bạn không thấy phát hiện, bạn có thể cần phải làm mới trang và/hoặc đợi vài phút.



13. Nhấp vào phát hiện UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS để xem chi tiết đầy đủ. Khi xem chi tiết phát hiện, bạn có thể thấy rằng đây là một phát hiện có mức độ nghiêm trọng HIGH. TPhát hiện này thông báo cho bạn về các nỗ lực thực hiện các thao tác API của AWS từ một máy chủ bên ngoài EC2, sử dụng thông tin xác thực tạm thời của AWS được tạo trên một EC2 instance trong tài khoản AWS của bạn.  Điều này cho thấy EC2 instance của bạn đã bị xâm phạm và thông tin xác thực tạm thời từ instance đã bị rò rỉ đến một máy chủ từ xa bên ngoài AWS.
![VPC](/images/6/6.1/s13.png)
![VPC](/images/6/6.1/s13b.png)
![VPC](/images/6/6.1/s13c.png)
#### Xem lại quá trình khắc phục tự động
Trong tình huống này, đã có thiết lập khắc phục tự động cho loại phát hiện này bằng cách sử dụng Amazon EventBridge và AWS Lambda. AWS Lambda là một dịch vụ tính toán serverless, dựa trên event cho phép bạn chạy code cho hầu hết các loại ứng dụng hoặc dịch vụ backend mà không cần cung cấp hoặc quản lý máy chủ. Bạn có thể kích hoạt Lambda từ hơn 200 dịch vụ AWS và chỉ trả tiền cho những gì bạn sử dụng. Amazon EventBridge là một serverless event bus giúp dễ dàng xây dựng các ứng dụng dựa trên event ở quy mô lớn bằng cách sử dụng các event được tạo từ ứng dụng của bạn và các dịch vụ AWS.  EventBridge cung cấp luồng dữ liệu theo thời gian thực từ các nguồn event đến các đích như AWS Lambda. Hãy cùng xem xét quá trình tự động này và xem nó đã hoạt động chưa!

14. Điều hướng đến bảng điều khiển Amazon EventBridge. Từ menu bên trái, trong phần Buses, nhấp vào Rules.



15. Nhấp vào tên rule đã được cấu hình cho loại phát hiện này, GuardDuty-Event-IAMUser-InstanceCredentialExfiltration. Hãy xem Event Pattern. Event patterns  có cùng cấu trúc với các event mà chúng khớp. Rules sử dụng event pattern để chọn các event và gửi chúng đến các mục tiêu. Một event pattern hoặc khớp với một event hoặc không. Tại đây, bạn có thể thấy chúng ta chỉ khớp các event với loại "UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS" từ "aws.guardduty".
![VPC](/images/6/6.1/s15.png)

Kết quả JSON:
```
{
  "source": ["aws.guardduty"],
  "detail": {
    "type": ["UnauthorizedAccess:IAMUser/InstanceCredentialExfiltration.OutsideAWS"]
  }
}
```

16.  Khi một event khớp với event pattern đã nêu ở trên, nó có thể được gửi đến các Target.  Nhấp vào tab Targets. Rule này được cấu hình để gửi những phát hiện này đến một hàm Lambda và một SNS topic.

![VPC](/images/6/6.1/s16.png)
17. Hãy kiểm tra hàm Lambda mà chúng ta đang gửi những phát hiện này đến. Truy cập vào bảng điều khiển Lambda và nhấp vào hàm có tên GDWorkshop-Remediation-InstanceCredentialExfiltration. Hàm Lambda này lấy tên Roletừ chi tiết phát hiện và sau đó gán một IAM policy hu hồi tất cả các phiên hoạt động cho role đó.

```
from __future__ import print_function
from botocore.exceptions import ClientError
import json
import datetime
import boto3
import os
 
def handler(event, context):
 
  # Log out event
  print("log -- Event: %s " % json.dumps(event))
 
  # Create generic function response
  response = "Error auto-remediating the finding."
 
  try:
 
    # Set Clients
    iam = boto3.client('iam')
    ec2 = boto3.client('ec2')
 
    # Set Role Variable
    role = event['detail']['resource']['accessKeyDetails']['userName']
 
    # Current Time
    time = datetime.datetime.utcnow().isoformat()
 
    # Set Revoke Policy
    policy = """
      {
        "Version": "2012-10-17",
        "Statement": {
          "Effect": "Deny",
          "Action": "*",
          "Resource": "*",
          "Condition": {"DateLessThan": {"aws:TokenIssueTime": "%s"}}
        }
      }
    """ % time
 
    # Add policy to Role to Revoke all Current Sessions
    iam.put_role_policy(
      RoleName=role,
      PolicyName='RevokeOldSessions',
      PolicyDocument=policy.replace('\n', '').replace(' ', '')
    )
 
    response = "GuardDuty Remediation | ID:%s: GuardDuty discovered EC2 IAM credentials (Role: %s) being used outside of the EC2 service.  All sessions have been revoked.  Please follow up with any additional remediation actions." % (event['detail']['id'], role)
 
  except ClientError as e:
    print(e)
 
  print("log -- Response: %s " % response)
  return response
```

#### Xác minh rằng quá trình khắc phục đã thành công
18. TĐể xác minh rằng phát hiện InstanceCredentialExfiltration đã được khắc phục, bạn có thể chạy lại một trong các lệnh CLI mà bạn đã chạy trước đó trên máy tính cục bộ của mình. Bạn sẽ thấy một phản hồi lỗi "with an explicit deny in an identity-based policy".

```aws dynamodb list-tables --profile badbob```

{{%notice info%}}
Lưu ý đến độc giả
Quá trình khắc phục có thể mất vài phút để hoàn thành.
{{%/notice%}}

22.  Bạn cũng có thể xem Role để đánh giá policy đã được đính kèm. Điều hướng đến [bảng điều khiển AWS IAM.](https://console.aws.amazon.com/iam/home?region=us-east-1).


23. Nhấp vào Roles trong menu bên trái.


24. Nhấp vào Role mà bạn đã xác định trong phát hiện GuardDuty, GDWorkshop-EC2-Compromised.


25. Trong tab Permissions, nhấp vào RevokeOldSessions policy. Chú ý đến lệnh từ chối rõ ràng tất cả.

![VPC](/images/6/6.1/s25.png)