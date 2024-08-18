---
title : "Sử dụng insights để ưu tiên và đo lường"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Khi nhóm của bạn bật các dịch vụ bảo mật AWS khác nhau và tổng hợp mọi thứ vào Security Hub, ban lãnh đạo yêu cầu bạn rút ra insight từ tất cả dữ liệu đó. Cụ thể, bạn được yêu cầu khai thác insight về sự tuân thủ, cảnh báo ưu tiên cao nhất, và các tài nguyên tạo ra số lượng cảnh báo bảo mật cao nhất.
{{%/notice%}}

Một insight trong AWS Security Hub là một tập hợp các phát hiện liên quan. Nó xác định một lĩnh vực bảo mật cần sự chú ý và can thiệp. Ví dụ, một insight có thể chỉ ra các EC2 instance đang bị phát hiện với các biện pháp bảo mật kém. Một insight tập hợp các phát hiện từ các nhà cung cấp phát hiện khác nhau.

Mỗi insight được định nghĩa bởi một câu lệnh nhóm và các bộ lọc tùy chọn. Câu lệnh nhóm chỉ định cách gom nhóm các phát hiện phù hợp, và xác định loại mục mà insight áp dụng. Ví dụ, nếu một insight được nhóm theo định danh tài nguyên, thì insight sẽ tạo ra danh sách các định danh tài nguyên. Các bộ lọc tùy chọn xác định các phát hiện phù hợp cho insight. Ví dụ, bạn có thể muốn chỉ xem các phát hiện từ các nhà cung cấp cụ thể hoặc các phát hiện liên quan đến các loại tài nguyên cụ thể.

Security Hub cung cấp một số insight được quản lý sẵn. Bạn không thể sửa đổi hoặc xóa các insight được quản lý.

Một insight chỉ trả về kết quả nếu bạn đã kích hoạt các tích hợp hoặc tiêu chuẩn tạo ra các phát hiện phù hợp. Ví dụ, insight được quản lý 29. Top resources by counts of failed CIS checks chỉ trả về kết quả nếu bạn kích hoạt tiêu chuẩn CIS AWS Foundations.

#### Xem các insight được quản lý sẵn
1. Trong Security Hub, điều hướng đến trang **Insights** bằng cách sử dụng thanh điều hướng bên trái. Dành một vài phút để xem các insight được quản lý sẵn.


2. Từ trang Insights, tìm insight có tên **Insight: 23. Top products by counts of findings**, và nhấp vào tiêu đề để mở. https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/arn:aws:securityhub:::insight/securityhub/default/28 

![VPC](/images/4/4.3/s2.png)

3. Insight này làm nổi bật số lượng phát hiện hoạt động được tổng hợp từ mỗi sản phẩm mà bạn đã tích hợp. Hiểu biết về mục đích sử dụng của bạn cho mỗi sản phẩm tích hợp này là hữu ích để đo lường nỗ lực của tổ chức bạn nhằm giảm số lượng lỗ hổng, mối đe dọa và vi phạm tuân thủ tài nguyên.
![VPC](/images/4/4.3/s3.png)


4. Từ trang Insights, tìm insight có tên **Insight: 24. Severity by counts of findings**, và nhấp vào tiêu đề để mở. https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/arn:aws:securityhub:::insight/securityhub/default/29 



5. Insight này làm nổi bật số lượng phát hiện hoạt động trong môi trường của bạn theo nhãn mức độ nghiêm trọng. Những con số này hữu ích cho việc báo cáo sự cải thiện dần dần trong môi trường của bạn. Cố gắng giảm những con số này theo thời gian, bắt đầu từ số lượng phát hiện có nhãn mức độ nghiêm trọng CRITICAL hoặc HIGH.
![VPC](/images/4/4.3/s5.png)

#### Tạo insight tùy chỉnh để theo dõi số lượng mối đe dọa hoạt động

6. Để theo dõi các vấn đề bảo mật đặc biệt cho môi trường AWS của bạn và việc sử dụng, bạn có thể tạo các insight tùy chỉnh. Bạn sẽ tạo các insight tùy chỉnh để theo dõi các chỉ số chính của tổ chức bạn. Insight đầu tiên bạn tạo sẽ làm nổi bật số lượng mối đe dọa hoạt động trong môi trường của bạn được nhóm theo mục đích mối đe dọa, loại tài nguyên bị ảnh hưởng, hoạt động độc hại tổng thể, v.v. Trở lại trang **Insights** và nhấp vào Create insight.


7. Nhấp vào **Add filter**. Dưới Filters, chọn **Product name**,  và sau đó nhập "GuardDuty" (lưu ý viết hoa). Chúng ta đang lọc các phát hiện từ GuardDuty vì đây là dịch vụ phát hiện mối đe dọa bạn đang sử dụng để liên tục theo dõi các tài khoản và khối lượng công việc AWS của bạn cho hoạt động độc hại.


8. Nhấp vào **Add filter** một lần nữa, nhưng lần này chọn **Group** dưới Grouping. Sau đó chọn **Type**. Một trong những thông tin hữu ích nhất được cung cấp cho các phát hiện của GuardDuty là loại phát hiện. Mục đích của loại phát hiện là cung cấp mô tả ngắn gọn nhưng dễ hiểu về vấn đề bảo mật tiềm ẩn. Bằng cách chọn "Group by Type", insight của bạn sẽ cung cấp số lượng các phát hiện phù hợp với mỗi loại phát hiện.


9. Nhấp vào **Create insight**. Đối với tên insight, nhập "Security alerts by GuardDuty finding type".
![VPC](/images/4/4.3/s9.png)

![VPC](/images/4/4.3/s9b.png)
10.  Bạn có thể nhấp vào **Insight details** để xem biểu đồ kết quả được trực quan hóa. Các insight cũng có thể được truy xuất qua API của Security Hub.
![VPC](/images/4/4.3/s10.png)

11.  Trở lại trang **Insights** liệt kê tất cả các insight của bạn.



12. Nhận thấy việc thiết lập này khá dễ dàng, bạn nên cân nhắc cách thực hiện tương tự trong mỗi tài khoản. Câu trả lời là sử dụng cơ sở hạ tầng dưới dạng code. Vì đó là cách bạn muốn thiết lập các insight dài hạn, hãy xóa insight tùy chỉnh mà bạn vừa tạo. Tìm kiếm danh sách insight cho "Security alerts by GuardDuty finding type".


13. Khi bạn tìm thấy nó, chọn biểu tượng ô nhỏ trên menu sau đó nhấp vào **Delete**.


#### CloudFormation để triển khai các insight tùy chỉnh trong Security Hub
{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Sau khi chứng minh rằng việc lấy insight từ Security Hub dễ dàng như thế nào, bạn đã được thách thức viết code cho cơ sở hạ tầng để triển khai các insight tùy chỉnh Security Hub cho tất cả các tài khoản của tổ chức bạn, để mỗi chủ tài khoản có thể thấy các insight giống nhau cho tài khoản cá nhân của họ mà nhóm bảo mật đang tổng hợp cho toàn bộ tổ chức.
{{%/notice%}}

AWS CloudFormation cho phép bạn mô hình hóa, cấp phát, quản lý các tài nguyên AWS và bên thứ ba bằng cách coi cơ sở hạ tầng dưới dạng mã. Đối với mục đích của mô-đun này, bạn có thể sử dụng code CloudFormation sau để triển khai thêm một số insight tùy chỉnh. Bạn có thể sửa đổi nó và thử nghiệm. Template ví dụ này giả định rằng Security Hub đã được kích hoạt sẵn. Để làm cho mô-đun này dễ dàng hơn một chút, CloudFormation template đã được chuẩn bị sẵn trong một bucket S3 cho bạn.


```
AWSTemplateFormatVersion: 2010-09-09
Description: Example template to create a Security Hub insights
Resources:
  SecurityHubInsightGDFindings:
    Type: 'AWS::SecurityHub::Insight'
    Properties:
      Name: Security alerts by GuardDuty finding type
      Filters:
        ProductName:
          - Value: GuardDuty
            Comparison: EQUALS
        WorkflowStatus:
          - Value: NEW
            Comparison: EQUALS
          - Value: NOTIFIED
            Comparison: EQUALS
        RecordState:
          - Value: ACTIVE
            Comparison: EQUALS
      GroupByAttribute: Type
  SecurityHubInsightSHControls:
    Type: 'AWS::SecurityHub::Insight'
    Properties:
      Name: Most Failed Security Hub Controls
      Filters:
        ProductName:
          - Value: Security Hub
            Comparison: EQUALS
        WorkflowStatus:
          - Value: NEW
            Comparison: EQUALS
          - Value: NOTIFIED
            Comparison: EQUALS
        RecordState:
          - Value: ACTIVE
            Comparison: EQUALS
      GroupByAttribute: GeneratorId
  SecurityHubInsightMostSev:
    Type: 'AWS::SecurityHub::Insight'
    Properties:
      Name: Resources with the most security alerts Sev40+
      Filters:
        SeverityNormalized:
          - Gte: 40
            Lte: 100
        ResourceType:
          - Value: AwsAccount
            Comparison: NOT_EQUALS
        WorkflowStatus:
          - Value: NEW
            Comparison: EQUALS
          - Value: NOTIFIED
            Comparison: EQUALS
        RecordState:
          - Value: ACTIVE
            Comparison: EQUALS
      GroupByAttribute: ResourceId
```
Template trên tạo ra 3 insight tùy chỉnh trong Security Hub:
+ Cảnh báo bảo mật theo loại phát hiện GuardDuty
+ Các kiểm soát Security Hub thất bại nhiều nhất
+ Tài nguyên có nhiều cảnh báo bảo mật nhất (mức độ nghiêm trọng medium-critical)

#### Triển khai CloudFormation Template của bạn với các insight tùy chỉnh

14. Vì CloudFormation trên đã được chuẩn bị sẵn trong S3, hãy mở trang [Stacks](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/?filteringText=cfn&filteringStatus=active&viewNested=false) trong bảng điều khiển CloudFormation.


15. Nhấp vào tên của stack, **cfn**.


16. Mở tab **Outputs**.
![VPC](/images/4/4.3/s16.png)


17.   Mở "InsightsCloudFormationLink" trong một tab mới để khởi chạy CloudFormation template đã cấu hình sẵn trong us-east-1 (N. Virginia). TĐiều này sẽ mở trang "Create stack" trong us-east-1 (N. Virginia) và điền liên kết đến CloudFormation template vào form. Đảm bảo rằng hiển thị "N. Virginia" ở góc trên bên phải của trang.



18. Nhấp vào **Create stack** ở cuối trang.
![VPC](/images/4/4.3/s18.png)


19. Chờ đợi cho template hoàn tất triển khai.



20. Trở lại Security Hub và mở trang **Insights**. 
![VPC](/images/4/4.3/s18b.png)
Tìm và xem các insight bạn vừa tạo bằng cách chuyển menu thả xuống ở đầu trang sang **Custom insights**. Ví dụ:

![VPC](/images/4/4.3/s18c.png)