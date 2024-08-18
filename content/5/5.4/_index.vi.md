---
title : "Building your own automated response"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 5.4 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: Quản lý của bạn đã yêu cầu bạn khám phá việc tự động hóa để khắc phục các phát hiện bảo mật, bắt đầu với các loại phát hiện yêu cầu thời gian khắc phục nhanh nhất.
{{%/notice%}}

Không giống như cách bạn đã thiết lập thông báo trước đó, bạn có thể không muốn tự động hóa hoàn toàn các phản hồi cho các phát hiện. Để thiết lập tự động hóa mà bạn có thể kích hoạt thủ công cho các phát hiện cụ thể, bạn có thể sử dụng **custom actions**. **Custom action** là một cơ chế của **Security Hub** để gửi các phát hiện đã chọn đến **EventBridge**, nơi có thể được khớp bởi một **EventBridge rule**. **Rule** này định nghĩa một hành động cụ thể cần thực hiện khi nhận được một phát hiện liên quan đến **custom action ID**. **Custom actions** có thể được sử dụng, ví dụ, để gửi một phát hiện cụ thể, hoặc một tập hợp nhỏ các phát hiện, đến một quy trình phản hồi hoặc khắc phục. Bạn có thể tạo tối đa 50 **custom actions**.

Trong phần này, chúng ta sẽ hướng dẫn bạn cách tạo một **custom action** trong **Security Hub** sẽ kích hoạt một **EventBridge rule** để thực thi một **Lambda function**. Mục đích của **Lambda function** là để thay đổi **Security Group** cho một **EC2 instance**. Đây chỉ là một ví dụ về cách xây dựng tự động hóa của riêng bạn. Ví dụ này chưa sẵn sàng cho môi trường sản xuất.

#### Create the Security Group

1. Điều hướng đến **EC2 console** và mở trang [Security Groups](https://console.aws.amazon.com/ec2/home?#SecurityGroups) bằng cách sử dụng bảng điều hướng bên trái.

2. Nhấp vào **Create Security Group**. Tại đây, bạn sẽ thiết lập **Security Group** mà bạn muốn áp dụng cho **instance** trong quá trình phản hồi sự cố. Bạn muốn giới hạn lưu lượng truy cập chỉ đến các địa chỉ cần thiết cho **CIDR** của công ty bạn. Đặt **Security group name** là "IncidentResponseSG" và cung cấp mô tả.

3. Xóa **VPC** được chỉ định và chọn **VPC** hiển thị "**cfn-GuardDutyLabs...**".

4. Xóa **outbound rule** được thêm mặc định.

5. Thêm 2 **inbound rules** để **CIDR** của công ty bạn có thể **RDP** và **SSH** vào **instance**. Làm theo ảnh chụp màn hình dưới đây.

{{%notice warning%}}
Các **CIDR blocks** chỉ là ví dụ giả để đại diện cho **CIDR** của công ty bạn nhằm cho phép nhóm bảo mật của bạn **RDP** và **SSH** vào **instance** để thực hiện phân tích pháp y.
{{%/notice%}}

6. Lưu **Security Group** bằng cách nhấp vào **Create Security Group**.

7. Sao chép **Security group ID** và **VPC ID** của **Security Group** mới của bạn. Bạn sẽ cần các **IDs** này để hoàn thành module này.
![VPC](/images/5/5.4/s6a.png)
Result:
![VPC](/images/5/5.4/s6b.png)
#### Create the Lambda function

8. Hãy tạo **Lambda function** mà chúng ta sẽ sử dụng để thay đổi **security group** trên **instance**. Đi đến https://console.aws.amazon.com/lambda/home?#/create/function?intent=authorFromScratch

9. Trên trang **Create function**, đặt **Function name** là "IR-ChangeInstanceSecurityGroup" và chọn **Node.js 20.x** cho **Runtime**.
![VPC](/images/5/5.4/s9.png)

10. Nhấp vào **Create function**.

11. Sao chép và dán đoạn mã sau vào trình soạn thảo được gắn nhãn **Code source** (xóa mã hiện có). Thay thế giá trị của **SecurityGroupId** (hiện đang được đặt thành "sg-XXXXXXXXXXXX") bằng **Security group ID** mà bạn đã ghi lại trước đó.

```
import { EC2Client, ModifyInstanceAttributeCommand } from "@aws-sdk/client-ec2";
const ec2Client = new EC2Client();

export const handler = async (event, context) => {
  try {
    const findings = event.detail.findings;
    const SecurityGroupId = "sg-XXXXXXXXXXXX";
    var instanceARN;
    if (findings && findings.length > 0){
        instanceARN = findings[0].Resources[0].Id;
    }
    
    const command = new ModifyInstanceAttributeCommand({
      InstanceId: instanceARN.split('/')[1],
      Groups: [SecurityGroupId]
    });

    await ec2Client.send(command);

    return 'Updated security group.';
  } catch (error) {
    console.error('Error:', error);
    throw error;
  }
};
```
12. Sau đó từ menu **Code source**, nhấp vào **File** rồi **Save**. Sau đó nhấp vào nút **Deploy**.
![VPC](/images/5/5.4/s12.png)

#### Update Lambda function permissions
13. Ngay phía trên phần **Code source** cho **Lambda function** bạn đã tạo, nhấp vào tab **Configuration**. Sau đó nhấp vào **Permissions** từ bảng điều hướng bên trái. Chúng ta cần sửa đổi vai trò thực thi cho **Lambda function** này.

14. Nhấp vào **Role name**. Tên này sẽ bắt đầu bằng "IR-ChangeInstanceSecurityGroup-role-". Điều này sẽ đưa bạn đến vai trò trong **Identity and Access Management (IAM) console**.
![VPC](/images/5/5.4/s14.png)

15. Từ trang **Role**, nhấp vào nút **Add permissions** và chọn **Create inline policy**.

16. Trên trang **Create policy**, nhấp vào tab **JSON**. Sau đó thay thế chính sách hiện tại bằng chính sách sau (không sử dụng chính sách này trong môi trường sản xuất, nó không tuân theo nguyên tắc least privilege).


```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Action": [
                "ec2:*",
                "ec2:Describe*",
                "ec2:ModifyInstanceAttribute"
            ],
            "Resource": "*",
            "Effect": "Allow"
        }
    ]
}
```
17. Nhấp vào nút **Next**. Nếu bạn thấy một biểu ngữ thông báo **You need permissions**, hãy bỏ qua nó.

18. Đặt tên cho chính sách của bạn là "ModifySG" và nhấp vào **Create policy**.
![VPC](/images/5/5.4/s18.png)

#### Create a Custom Action in Security Hub
19. Mở **Security Hub**. Mở trang **Custom actions** dưới mục [Management](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/settings/actions) trong bảng điều hướng bên trái.

20. Nhấp vào nút **Create custom action**.

21. Nhập **Action Name**, **Action Description**, và **Action ID** đại diện cho hành động mà bạn đang triển khai, ví dụ, "UpdateSecurityGroup".

22. Nhấp vào **Create custom action**.
![VPC](/images/5/5.4/s22.png)

![VPC](/images/5/5.4/s22b.png)

23. Sao chép **Custom action ARN** đã được tạo. Bạn sẽ cần **Custom ARN** trong các bước tiếp theo.

#### Create Amazon EventBridge Rule to capture the Custom Action
Trong phần này, bạn sẽ định nghĩa một **EventBridge rule** để khớp với các sự kiện (findings) từ **Security Hub** được chuyển tiếp bởi **custom action** mà bạn đã định nghĩa ở trên.

24. Điều hướng đến **Amazon EventBridge console**.

25. Nhấp vào **Create rule** ở phía bên phải.

26. Trên trang **Define rule detail**, đặt tên và mô tả cho rule của bạn để thể hiện mục đích của rule (ví dụ, cùng tên và mô tả bạn đã sử dụng cho **custom action**). Sau đó nhấp vào **Next**.

27. Tất cả các findings của **Security Hub** được gửi dưới dạng sự kiện đến **AWS default event bus**. Phần **define pattern** cho phép bạn xác định các bộ lọc để thực hiện một hành động cụ thể khi các sự kiện khớp xuất hiện. Ở bước **Build event pattern**, để **Event source** được đặt là **AWS events or EventBridge partner events**.

28. Cuộn xuống **Event pattern**. Dưới **Event source**, giữ nguyên **AWS Services**, và dưới **AWS Service**, chọn **Security Hub**.

29. Đối với **Event Type**, chọn **Security Hub Findings – Custom Action**.

30. Sau đó chọn nút radio **Specific custom action ARN(s)** và nhập **ARN** cho **custom action** mà bạn đã tạo trước đó.

31. Lưu ý rằng khi bạn chọn các tùy chọn này, **Event pattern** ở bên phải đã được cập nhật. Nhấp vào **Next**.

32. Ở bước **Select target(s)**, chọn **Lambda function** từ danh sách thả xuống **Select a target**. Sau đó chọn **IR-ChangeInstanceSecurityGroup** từ danh sách thả xuống **Function**.

33. Nhấp vào **Next**.

34. Trên bước **Configure tags**, nhấp vào **Next**.

35. Trên bước **Review and create**, nhấp vào **Create rule**.
![VPC](/images/5/5.4/s35.png)

#### Trigger the automation
36. Tự động hóa của bạn bây giờ đã được thiết lập. Hãy thử nghiệm nó trên một finding cho một **EC2 instance**. Quay lại trang **Findings** trong **Security Hub**.

37. Thêm một bộ lọc cho **Resource Type** và nhập **AwsEc2Instance** (phân biệt chữ hoa chữ thường). Nhấp vào **Apply**.
![VPC](/images/5/5.4/s37.png)

38. Thêm một bộ lọc khác cho **EC2 Instance VPC ID** và nhập **VPC ID** mà bạn đã ghi lại khi tạo **Security Group** ở đầu module này. Nhấp vào **Apply**.

39. Nhấp vào tiêu đề của bất kỳ finding nào trong danh sách được lọc này mà tài nguyên hiển thị loại **AwsEc2Instance**.

40. Trước khi chúng ta kích hoạt tự động hóa, hãy xem cấu hình hiện tại của **EC2 instance** để xác nhận tự động hóa của chúng ta hoạt động. Mở rộng phần **Resources** của finding.
![VPC](/images/5/5.4/s40.png)

41. Nhấp vào liên kết màu xanh cho **EC2 instance** này, dưới tiêu đề **Resource ID**. Điều này sẽ mở một tab mới hiển thị trên **EC2 console** chỉ hiển thị **EC2 instance** này.

42. Nhấp vào **instance ID**, sau đó nhấp vào tab **Security**. Ghi lại tên của **security group**.
![VPC](/images/5/5.4/s42.png)

43. Quay lại tab **Security Hub** trong trình duyệt của bạn (đừng đóng tab **EC2**, bạn sẽ cần quay lại đây) và nhấp vào hộp kiểm bên trái của finding này.

44. Trong menu thả xuống **Actions**, chọn tên của **custom action** của bạn, **Update SG**.
![VPC](/images/5/5.4/s44.png)

45. Sau khi thấy biểu ngữ hiển thị "Successfully started action...", quay lại tab **EC2** trên trình duyệt. Làm mới tab (thường sau 3 phút). Xác minh rằng **security group** trên **instance** đã được thay đổi.
![VPC](/images/5/5.4/s45.png)
