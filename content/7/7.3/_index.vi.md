---
title : "Tích hợp Amazon Inspector vào quy trình CI/CD"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

Trong mô-đun này, bạn sẽ học cách tích hợp quét container image của Amazon Inspector trực tiếp vào quy trình CI/CD của bạn để quét các lỗ hổng phần mềm và cung cấp báo cáo vào cuối quá trình xây dựng của bạn. Tính năng này cho phép khách hàng điều tra và khắc phục các rủi ro trước khi triển khai.

Tích hợp CI/CD của Amazon Inspector sử dụng kết hợp giữa Amazon Inspector SBOM Generator và Amazon Inspector Scan API để tạo ra các báo cáo lỗ hổng cho container image của bạn. Amazon Inspector SBOM Generator tạo một hóa đơn vật liệu phần mềm (software bill of materials - SBOM) từ container image được cung cấp, sau đó, Amazon Inspector Scan API quét SBOM đó và tạo ra một báo cáo chi tiết về bất kỳ lỗ hổng nào được phát hiện.

Bạn có thể thực hiện tích hợp CI/CD với Amazon Inspector thông qua các plugin Amazon Inspector được xây dựng đặc biệt cho các giải pháp CI/CD riêng lẻ và có sẵn trên marketplace của họ, hoặc bạn có thể tạo tích hợp quét tùy chỉnh của riêng mình.

Sử dụng plugin Jenkins để tích hợp Amazon Inspector vào quy trình CI/CD của bạn. Bạn sẽ sử dụng các image nằm trong Docker Hub.

#### Cài đặt Jenkins trên một EC2 instance
Trong phần này, bạn sẽ cài đặt Jenkins và Trình tạo SBOM của Amazon Inspector trên phiên bản EC2. Jenkins là máy chủ tự động hóa nguồn mở tích hợp với một số Dịch vụ AWS, bao gồm: AWS CodeCommit, AWS CodeDeploy, Amazon EC2 Spot và Amazon EC2 Fleet. Trình tạo SBOM của Amazon Inspector (Sbomgen) là một công cụ nhị phân tạo ra hóa đơn vật liệu phần mềm (SBOM) cho một container image. SBOM là bản kiểm kê được thu thập của phần mềm được cài đặt trên hệ thống. Sbomgen hoạt động bằng cách quét các tệp được biết có chứa thông tin về các gói đã cài đặt. Nếu tìm thấy một trong những tệp này, công cụ sẽ trích xuất tên gói, phiên bản và siêu dữ liệu khác.

1. Điều hướng đến bảng điều khiển Amazon EC2 và mở trang Instances. https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running 



2. Chọn Jenkins instance và nhấp vào nút Connect ở đầu trang.



3. Chuyển sang tab Session Manager trên trang Connect to instance.



4. Nhấp vào Connect. Thao tác này sẽ mở một phiên làm việc của Systems Manager.



5. Tải xuống kho lưu trữ Jenkins.
```sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo ```

6. Nhập một tệp khóa từ Jenkins để cho phép cài đặt.
```sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key```


7. Nâng cấp bất kỳ thành phần nào của Jenkins.
```sudo yum upgrade```


8. Cài đặt Java.
```sudo yum install java-11-amazon-corretto -y```



9. Cài đặt Jenkins.
```sudo yum install jenkins -y```


10. Kích hoạt dịch vụ Jenkins để khởi động cùng hệ thống.
```sudo systemctl enable jenkins```


11. Bắt đầu dịch vụ Jenkins.
```sudo systemctl start jenkins```



12. Xác minh rằng dịch vụ Jenkins đang chạy.
```sudo systemctl status jenkins```

![VPC](/images/7/7.3/s12.png)
13. Nhấp vào Terminate.


14.	Quay lại trang Instances trong [bảng điều khiển EC2](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running;tag:Name=Jenkins).  


15.	Chọn Jenkins instance.


16.	Đi đến tab Security trong chi tiết instance.


17.	Ghi chú lại IAM Role trong phần Security details. Bạn sẽ tra cứu ARN của Role này trong mô-đun sau.


18.	Trong Security groups, nhấp vào security group được gắn với instance. Nó sẽ có tên bắt đầu bằng "sg-".


19.	Bạn sẽ được chuyển đến cấu hình của security group. Nhấp vào nút Edit inbound rules.


20.	Bạn sẽ cần kết nối với instance EC2 qua cổng 8080 để truy cập trang quản trị của Jenkins. Nhấp vào nút Add rule.


21.	Đối với rule mới, đặt Type thành Custom TCP. Đặt Port range thành "8080". Đối với Source, chọn My IP.


22.	Nhấp vào Save rules.
![VPC](/images/7/7.3/s22.png)

#### Cấu hình IAM role cho tích hợp CI/CD của Jenkins
23. Chúng ta cần tạo IAM role cho phép truy cập Amazon Scan API, để quét danh sách phần mềm, trong Jenkins. Mở bảng điều khiển IAM https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/home 



24. Từ thanh điều hướng của bảng điều khiển IAM, mở Policies.



25. Nhấp vào Create Policy.


26. Trong Policy Editor nhấp vào nút JSON, và thay thế policy bằng đoạn mã sau:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "inspector-scan:ScanSbom",
            "Resource": "*"
        }
    ]
}
```

27.	Nhấp vào Next.


28.	Trên trang Review and create, nhập "InspectorCICDscan-policy" cho Policy name. Policy này sẽ được gán cho role mà bạn sẽ tạo trong vài bước nữa.
![VPC](/images/7/7.3/s28.png)

29.	Nhấp vào Create policy.


30.	Từ ngăn điều hướng của bảng điều khiển IAM, mở Roles và tìm kiếm role mà bạn đã ghi chú trước đó (gắn với instance EC2, Jenkins). Role này sẽ bắt đầu bằng "cfn-InspectorLabsInfrastructure" và bao gồm term "SSMInstanceRole". Nhấp vào tên role để mở chi tiết.


31.	Sao chép ARN của role. Bạn sẽ cần thông tin này trong vài bước nữa.


32.	Từ thanh điều hướng của bảng điều khiển IAM, mở Roles và sau đó nhấp vào Create Role.


33.	Đối với Trusted entity type, chọn Custom trust policy.


34.	Thay thế Custom trust policy bằng đoạn mã sau. Thay thế "ARN" bằng ARN của IAM role bạn đã sao chép trước đó (có chứa cụm "SSMInstanceRole").
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "ARN"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

![VPC](/images/7/7.3/s34.png)
35. Nhấp vào Next.



36. Trong Add permissions tìm kiếm và chọn policy mà bạn đã tạo trước đó, "InspectorCICDscan-policy". Sau đó nhấp vào Next.


37. Đặt tên cho role là "InspectorCICDscan-role". Sau đó nhấp vào Create Role.

![VPC](/images/7/7.3/s37.png)
{{%notice tip%}}
Cấu hình IAM permission này được sử dụng cho workshop, nhưng có thể không áp dụng cho môi trường của bạn. Trong workshop, role được gán cho EC2 instance lưu trữ Jenkins, được ủy quyền cho role InspectorCICDscan-role mới được tạo. InspectorCICDscan-role sẽ được Jenkins sử dụng. Do đó, khi Jenkins sử dụng InspectorCICDscan-role, nó sẽ đảm nhận IAM role được gán cho EC2 instance, role có quyền truy cập vào dịch vụ Inspector Scan.
{{%/notice%}}

#### Cấu hình Jenkins
38. Bây giờ instance đã có security group và AWS permission chính xác, bạn có thể cấu hình Jenkins để sử dụng Inspector để quét lỗ hổng. Quay lại bảng điều khiển EC2 và mở [trang Instances](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running;tag:Name=Jenkins). 



39. Chọn Jenkins instance.


40. Nhấp vào nút Connect ở đầu trang.


41. Dưới tab Session Manager, chọn Connect.



42. Chúng ta sẽ cần lấy mật khẩu quản trị viên ban đầu để đăng nhập vào Jenkins.
```sudo cat /var/lib/jenkins/secrets/initialAdminPassword```

![VPC](/images/7/7.3/s42.png)
43. Sao chép kết quả. Đây là mật khẩu quản trị viên ban đầu của Jenkins. Bạn sẽ cần thông tin này ở bước sau.


44. Nhấp vào Terminate.



45. Quay lại bảng điều khiển EC2 và chọn [Jenkins instance](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running;tag:Name=Jenkins).  



46. Trong tab Details, sao chép địa chỉ public IP dưới Public IPv4 address.

47. Trong một tab trình duyệt mới, dán địa chỉ IP và thêm ":8080" vào. Nó sẽ trông tương tự như "xx.xx.xx.xx:8080".
![VPC](/images/7/7.3/s47.png)

48. Nhập mật khẩu quản trị viên ban đầu từ trước đó, sau đó nhấp vào Continue.
![VPC](/images/7/7.3/s48.png)


49. Từ trang Customize Jenkins, nhấp vào Install suggested plugins.


50. Sau khi cài đặt hoàn tất, trang Create First Admin User sẽ mở ra. Nhập thông tin của bạn, sau đó chọn Save and Continue.
![VPC](/images/7/7.3/s50.png)


51. Trên trang Instance Configuration, nhấp vào Save and Finish.



52. Trên trang hiển thị Jenkins đã sẵn sàng,  nhấp vào Start using Jenkins.



53. Từ thanh điều hướng, mở Manage Jenkins.
![VPC](/images/7/7.3/s53.png)


54. Trong System Configuration, mở Plugins.



55. Chọn Available plugins từ menu.



56. Tìm kiếm các plugin có sẵn cho Amazon Inspector Scanner.



57. Chọn ô bên cạnh Amazon Inspector Scanner, sau đó nhấp vào Install.

![VPC](/images/7/7.3/s57.png)


58. Chờ cho quá trình cài đặt hoàn tất. Ở cuối trang, nhấp vào Go back to the top page.


59. Từ thanh điều hướng, nhấp vào Manage Jenkins.


60. Trong System Configuration, nhấp vào Nodes.



61. Nhấp vào Built-In Node.
![VPC](/images/7/7.3/s61.png)


62. Mở Script Console từ thanh điều hướng.


63. Trong hộp văn bản, thêm dòng sau:
```System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")```

64. Nhấp vào Run.
![VPC](/images/7/7.3/s64.png)

#### Build một Jenkins job
65.	Đi đến bảng điều khiển Jenkins bằng cách sử dụng liên kết ở góc trên bên trái, và chọn Create a job.
![VPC](/images/7/7.3/s65.png)

66.	Trong mục Enter an item name, nhập "InspectorScan".

67.	Chọn Freestyle project, sau đó nhấp vào OK.
![VPC](/images/7/7.3/s67.png)

68.	Cuộn xuống phần Build Steps, nhấp vào dropdown Add build step, và chọn Amazon Inspector Scan.


69.	Chọn Automatic cho phương pháp cài đặt Inspector-sbomgen, sau đó chọn Linux, AMD64.


70.	Đối với Image Id, nhập "centos:latest".


71.	Đối với AWS Region, chọn us-east-1.

72.	Trong một tab mới, mở IAM console và tìm kiếm role bạn đã tạo trước đó có tên là InspectorCICDscan-role. Sao chép ARN. https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles/details/InspectorCICDscan-role 


73.	Dán ARN của IAM role vào trang cấu hình build của Jenkins.
![VPC](/images/7/7.3/s73.png)

74.	Nhấp vào Save.


75.	Thao tác này sẽ đưa bạn đến một bảng điều khiển cho Jenkins build job của bạn. Từ thanh điều hướng, nhấp vào Build Now để tạo build đầu tiên của bạn. Nếu bạn gặp vấn đề với build, hãy thử nhấp vào Build Now một lần nữa hoặc hỏi người hỗ trợ của bạn để được giúp đỡ.
![VPC](/images/7/7.3/s75.png)
#### Xem xét build để tìm các lỗ hổng bảo mật
76. Trong Build History, tìm kiếm dấu màu xanh lá cây để chỉ ra một build thành công. Nhấp vào dấu màu xanh lá cây.
![VPC](/images/7/7.3/s76.png)

77. Điều này sẽ đưa bạn đến phần đầu ra của Jenkins job. Bạn có thể thấy rằng Inspector đã quét image và cung cấp các liên kết đến các báo cáo. Nhấp vào liên kết bên cạnh Build Artifacts.
![VPC](/images/7/7.3/s77.png)


78. Nhấp vào index.html. Nếu có bất kỳ lỗ hổng nào, chúng sẽ hiển thị trong báo cáo.

![VPC](/images/7/7.3/s78.png)