---
title : "Ứng phó với mã độc trên Amazon Elastic Block Store"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 6.5 </b> "
---
{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Bạn được giao nhiệm vụ xác định (và điều tra) xem GuardDuty có phát hiện mã độc trên các ổ Amazon Elastic Block Store (Amazon EBS) được gắn vào các Amazon Elastic Compute Cloud (Amazon EC2) instance và khối công việc container hay không.
{{%/notice%}}

1. Trước khi bắt đầu, hãy xác nhận rằng Malware Protection đã được thiết lập trong tài khoản của bạn. Điều hướng đến bảng điều khiển GuardDuty và mở trang Malware Protection.


2. Xác nhận rằng "GuardDuty-initiated malware scan is enabled". Malware Protection giúp bạn phát hiện sự hiện diện tiềm ẩn của mã độc bằng cách quét các ổ Amazon Elastic Block Store (Amazon EBS) được gắn vào các Amazon Elastic Compute Cloud (Amazon EC2) instance và khối công việc container.


3. GuardDuty cung cấp cho bạn tùy chọn giữ lại các snapshot của các ổ EBS trong tài khoản AWS của bạn khi phát hiện mã độc. Theo mặc định, cài đặt giữ lại các snapshot sẽ bị tắt. Snapshot sẽ chỉ được giữ lại nếu bạn đã bật cài đặt này trước khi quá trình quét bắt đầu. Nếu bạn bật cài đặt giữ lại snapshot cho tài khoản của mình, khi mã độc được tìm thấy và snapshots được giữ lại, bạn sẽ phải chịu chi phí sử dụng cho snapshot. Chuyển sang "Retain scanned snapshots when malware is detected" trong General settings.


#### Điều tra mã độc được GuardDuty phát hiện
Malware Protection cung cấp hai loại quét để phát hiện hoạt động có khả năng độc hại trong các Amazon EC2 instance và khối công việc container của bạn – Quét mã độc do GuardDuty khởi xướng và Quét mã độc theo yêu cầu. Quét mã độc do GuardDuty khởi xướng được kích hoạt khi GuardDuty phát hiện hành vi đáng ngờ cho thấy mã độc trên Amazon EC2 instance hoặc khối công việc container. Xem lại các phát hiện kích hoạt quét mã độc do GuardDuty khởi xướng để tìm hiểu về các phát hiện của GuardDuty có thể khởi động quá trình quét mã độc. Những phát hiện này được gọi là phát hiện kích hoạt trong workshop này. Một phát hiện Malware Protection mới được tạo cho mỗi lần quét phát hiện mã độc.

4. Mở trang Findings trong GuardDuty.


5. Trong trường Add filter criteria chọn Finding Type và nhập "Execution:EC2/MaliciousFile". Nhấp vào Apply.
![VPC](/images/6/6.5/s5a.png)
Kết quả:
![VPC](/images/6/6.5/s5b.png)
6. Nhấp vào một trong các phát hiện phù hợp để xem chi tiết. Dành vài phút để xem lại phát hiện và xác định EC2 instance bị ảnh hưởng, đường dẫn tệp mã độc và tên tệp cũng như các thông tin khác về phát hiện

![VPC](/images/6/6.5/s6.png)
Instance bị ảnh hưởng: i-0c79413d9f0d9941e

![VPC](/images/6/6.5/s6b.png)
Đường dẫn tệp mã độc /eicar.com

![VPC](/images/6/6.5/s6c.png)
Tên tệp mã độc: eicar.com

7. Nếu bạn cập nhật bộ lọc Loại phát hiện thành "Execution:ECS/MaliciousFile", bạn sẽ thấy các phát hiện mã độc cho container.

8. Nhấp vào liên kết "Scan ID" từ chi tiết phát hiện để xem thêm chi tiết về quá trình quét. Liên kết sẽ đưa bạn đến trang quét mã độc. Nhấp lại vào scan ID. Tại đây, bạn sẽ thấy các chi tiết bổ sung như kích thước ổ được quét, ổ bị nhiễm và thời gian chạy quá trình quét. Bạn cũng sẽ thấy ID phát hiện của phát hiện kích hoạt đã kích hoạt quá trình quét.
![VPC](/images/6/6.5/s8a.png)
![VPC](/images/6/6.5/s8b.png)
Xem phát hiện:
```
{
  "DetectorId": "c8c860de84297606e66fe1417c3ba44a",
  "AdminDetectorId": "c8c860de84297606e66fe1417c3ba44a",
  "ScanId": "4b879e92a6a795433e66f10c577ef4e6",
  "ScanStatus": "COMPLETED",
  "ScanStartTime": "2024-07-17T17:50:12.000Z",
  "ScanEndTime": "2024-07-17T18:02:01.000Z",
  "TriggerDetails": {
    "GuardDutyFindingId": "72c860e606f07b17804e7bf1b92dd4e4"
  },
  "ResourceDetails": {
    "InstanceArn": "arn:aws:ec2:us-east-1:486547846362:instance/i-0c79413d9f0d9941e"
  },
  "ScanResultDetails": {
    "ScanResult": "INFECTED"
  },
  "AccountId": "486547846362",
  "TotalBytes": 2619136718,
  "FileCount": 50898,
  "AttachedVolumes": [
    {
      "VolumeArn": "arn:aws:ec2:us-east-1:486547846362:volume/vol-0179bcf64ae7ccba7",
      "VolumeType": "gp3",
      "DeviceName": "/dev/xvda",
      "VolumeSizeInGB": 8,
      "EncryptionType": "UNENCRYPTED",
      "SnapshotArn": "arn:aws:ec2:us-east-1:486547846362:snapshot/snap-0c07cebc08b3eae39"
    }
  ],
  "ScanType": "GUARDDUTY_INITIATED"
}
```

{{%notice tip%}}
Các giá trị có thể có cho trạng thái quét là Completed, Running, Skipped, và Failed. Sau khi quá trình quét hoàn tất, Kết quả quét được điền cho các lần quét có Trạng thái là Completed.  Các giá trị có thể có cho Kết quả quét là Clean và Infected. Sử dụng Scan type, bạn có thể xác định liệu quá trình quét mã độc do GuardDuty khởi xướng hay theo yêu cầu.
{{%/notice%}}

9. Chọn liên kết bên cạnh ID phát hiện. Điều này sẽ đưa bạn đến một trang mới chỉ hiển thị phát hiện kích hoạt. Sau đó, chọn phát hiện và khám phá chi tiết về phát hiện kích hoạt. Phát hiện kích hoạt cũng có một phần tiêu đề Quét mã độc liệt kê các chi tiết về quá trình quét.


{{%notice tip%}}
Đối với mỗi Amazon EC2 instance và khối công việc container mà GuardDuty tạo ra các phát hiện, quá trình quét mã độc do GuardDuty khởi xướng tự động được kích hoạt mỗi 24 giờ. Nếu nhiều phát hiện kích hoạt được tạo trong khoảng thời gian 24 giờ đối với một EC2 instance, chỉ phát hiện kích hoạt đầu tiên mới khởi động quá trình quét.
{{%/notice%}}


10.   Nếu đây là EC2 instance duy nhất bị xâm nhập, bạn có thể tiến hành điều tra và cô lập instance theo một kịch bản phản ứng sự cố (điều này nằm ngoài phạm vi của mô-đun này). Nếu bạn đã bật cài đặt giữ lại snapshot cho các ổ EBS, các snapshot này sẽ được giữ lại trong tài khoản của bạn chỉ khi mã độc được tìm thấy, bạn có thể sử dụng các snapshot này để điều tra thêm.

#### [Tùy chọn] Khởi động quét mã độc theo yêu cầu
Nếu bạn muốn phát hiện sự hiện diện của mã độc trong các instance Amazon EC2 theo yêu cầu, bạn có thể khởi động quá trình quét mã độc theo yêu cầu bằng cách cung cấp Amazon Resource Name (ARN) của Amazon EC2 instance mà bạn muốn quét. Bạn cũng có thể chạy quét mã độc theo yêu cầu sau khi đã xử lý các tệp do GuardDuty xác định là một phần của phát hiện mã độc trước đó và muốn xác minh tệp đó không còn tồn tại.

11.  Từ trang Findings, mở một trong các phát hiện Execution:EC2/MaliciousFile để xem lại chi tiết.


12. Bạn cần Amazon Resource Name (ARN) của instance EC2 để khởi động quá trình quét mã độc theo yêu cầu. ARN có dạng "arn:aws:ec2:{region}:{account ID}:instance/{instance ID}". Hãy xây dựng ARN bằng thông tin từ phát hiện.
![VPC](/images/6/6.5/s12.png)

{{%notice tip%}}
ID tài nguyên của bạn sẽ có dạng "arn:aws:ec2:us-east-1:012345678901:instance/i-123456xxyyzzaaabb2"
{{%/notice%}}


13. Mở trang Malware scans từ thanh điều hướng bên trái.


14. Nhấp vào nút Start new on-demand scan.


15. Nhập ARN của instance EC2, sau đó nhấp vào Confirm.
![VPC](/images/6/6.5/s15.png)

16.  Bây giờ làm mới trang, bạn sẽ thấy một ID quét mới với loại quét là ‘On demand’ với trạng thái quét là ‘Running’.

![VPC](/images/6/6.5/s16.png)

![VPC](/images/6/6.5/s16b.png)