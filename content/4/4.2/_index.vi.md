---
title : "Ẩn các phát hiện quy mô lớn bằng tự động hóa"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

{{%notice info%}}
**Tình huống / Báo cáo vấn đề**: Sau khi tập trung tất cả các cảnh báo bảo mật của bạn và bắt đầu ưu tiên các phát hiện và kiểm soát để tập trung vào trước tiên, nhóm của bạn đã quyết định rằng có quá nhiều phát hiện. Trong nỗ lực giảm nhiễu, bạn đã được giao nhiệm vụ thiết lập các quy tắc tạm thời để ẩn các phát hiện có mức độ ưu tiên quá thấp để tập trung vào lúc này.
{{%/notice%}}

#### Tự động hóa việc ẩn các phát hiện để giảm nhiễu
Cuối cùng, chúng ta sẽ tạo một quy tắc để ẩn các phát hiện. Khi bạn đang sử dụng Security Hub, đặc biệt nếu bạn có một môi trường lớn hoặc sử dụng nhiều tích hợp có sẵn trong Security Hub, bạn có thể thấy cần thiết phải giảm khối lượng các phát hiện thông qua việc ẩn chúng. Điều này chỉ được khuyến nghị nếu nó giúp bạn tập trung vào các phát hiện quan trọng nhất. Nếu bạn thấy cần thiết phải sử dụng quy tắc ẩn, bạn chỉ nên ẩn các phát hiện có mức độ ưu tiên thấp, và bạn nên thực hiện điều này như một biện pháp tạm thời và xem xét lại quy tắc ẩn sau khi bạn đã tiến bộ trong việc cải thiện tình trạng bảo mật của mình và giảm số lượng các phát hiện.


48. Quay lại trang **Automations** trong Security Hub.


49. Nhấp vào **Create rule**.


50. Chọn **Create custom rule**.


51. Thay đổi **tên Rule** thành "Suppress low severity Inspector findings".



52. Sao chép tên Rule vào **Rule description** hoặc nhập mô tả của riêng bạn.


53. Đối với **Criteria** đầu tiên, chọn **ProductName** cho **Key**. Chọn **EQUALS** cho **Operator**. Nhập "Inspector" cho **Value**.


54. Nhấp vào **Add criteria**.



55. Đối với **Criteria** thứ hai, chọn **SeverityLabel** cho **Key**. Chọn **EQUALS** cho **Operator**. Chọn **INFORMATIONAL** cho **Value**.



56. Nhấp vào **Add another value** dưới tiêu chí bạn vừa thêm.
![VPC](/images/4/4.2/s56.png)


57. Một lần nữa, chọn **EQUALS** cho **Operator**. Chọn **LOW** cho **Value**. Bây giờ rule sẽ áp dụng cho các phát hiện từ Inspector với nhãn mức độ nghiêm trọng là INFORMATIONAL hoặc LOW.



58. Nhấp vào **Refresh** để xem trước các phát hiện phù hợp.
![VPC](/images/4/4.2/s58.png)

59. Dưới **Automated action**, chọn **SUPPRESSED** cho **Workflow status**.



60. Cuộn xuống **Note**, và nhập "Automatically suppressed via automation."
![VPC](/images/4/4.2/s60.png)

61. Giữ nguyên các cài đặt mặc định khác và nhấp vào **Create rule**.  Nếu bạn dự định ẩn các phát hiện, bạn nên xem xét lại các rule ẩn theo lịch trình để đảm bảo chúng vẫn cần thiết và chấp nhận được đối với tổ chức của bạn. Nếu cần, bạn có thể chỉnh sửa hoặc xóa các quy tắc tự động hóa.


62. Thử xóa một rule. Từ trang Automations, chọn ô bên cạnh rule có tên "Suppress low severity Inspector findings".

63. Nhấp vào menu thả xuống **Action**, và chọn **Delete**. 

![VPC](/images/4/4.2/s62a.png)
Khi hộp thoại xác nhận xuất hiện, nhấp vào **Delete** lần nữa.
![VPC](/images/4/4.2/s62b.png)