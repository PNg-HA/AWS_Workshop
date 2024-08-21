---
title : "Tự động hóa uu tiên các phát hiện quy mô lớn"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Tự động nâng cao mức độ nghiêm trọng của các phát hiện
Bước đầu tiên bạn sẽ thiết lập là nâng cao mức độ nghiêm trọng của các phát hiện ưu tiên cho các ứng dụng quan trọng trong doanh nghiệp. Cụ thể, chúng ta muốn nâng cao mức độ nghiêm trọng của tất cả các phát hiện từ GuardDuty cho các EC2 instances được gán nhãn cho môi trường sản xuất.

1. Mở trang **Automations** trong Security Hub.


2. Chọn **Create rule**.


3. Đối với quy tắc tự động hóa này, chúng ta sẽ bắt đầu với một template. Giữ "Create a rule from template" được chọn và dưới **Rule template** chọn **Elevate severity of findings that relate to important resources**.
![VPC](/images/4/4.1/s3.png)

4. Thay đổi **Rule name** thành "Elevate severity of GuardDuty findings that relate to production EC2 instances".


5. Sao chép tên Rule vào **Rule description** hoặc nhập mô tả của riêng bạn.


6. Dành một chút thời gian để xem lại rule. Cần thay đổi một số tiêu chí cho trường hợp sử dụng của chúng ta.



7. Thay đổi **Value** của tiêu chí đầu tiên (Key: ProductName) thành "GuardDuty".


8. Chọn **Remove** bên cạnh tiêu chí, Key: ComplianceStatus.


9. Chọn **Remove** bên cạnh tiêu chí cuối cùng (Key: ResourceId).



10. Chọn **Add criteria**. Đối với **Key** chọn **ResourceTags**. Đối với **Operator** chọn **EQUALS**. Đối với **Value**, nhập "Environment" vào ô trên và "Production" và ô dưới (đây là cặp key-value cho tag).
![VPC](/images/4/4.1/s10.png)

11.  Trong **Preview of findings that match criteria** chọn **Refresh** để đảm bảo bạn đã cấu hình đúng mọi thứ. Bạn sẽ thấy một bản xem trước các phát hiện hiện có đáp ứng tiêu chí.


12. Đối với rule này, chúng ta sẽ nâng cao tất cả các phát hiện phù hợp lên mức độ nghiêm trọng HIGH . Nếu thực hiện điều này cho tổ chức của bạn, bạn có thể muốn thêm một quy tắc khác để nâng cao các phát hiện từ GuardDuty mà ban đầu là HIGH lên mức độ nghiêm trọng CRITICAL. Trong phần **Automated action**, đặt Severity thành **HIGH**.



13. Giữ các cài đặt mặc định khác và chọn **Create rule**. Kết quả:
![VPC](/images/4/4.1/s13.png)

#### Tự động thêm các trường do người dùng định nghĩa vào cảnh báo sản xuất
Có những trường hợp bạn có thể muốn xác định các cảnh báo trên các tài nguyên sản xuất, nhưng không nâng cao mức độ nghiêm trọng. Một cách tiếp cận để thực hiện điều này là thêm ghi chú và các trường do người dùng định nghĩa. Quy tắc tự động hóa tiếp theo bạn sẽ thiết lập là thêm ghi chú và các trường do người dùng định nghĩa vào bất kỳ phát hiện nào từ các tài khoản chúng tôi xác định là môi trường sản xuất.

14. Quay lại trang **Automations** trong in Security Hub.


15. Chọn **Create rule**.


16. Đối với quy tắc tự động hóa này, chúng ta sẽ chọn **Create custom rule**.


17. Thay đổi **tên Rule** thành "Tag production findings".


18. Thay đổi **Rule description** thành "Tag findings for resources in production accounts".


19. Đối với rule này, chúng ta chỉ cần một tiêu chí. Đặt **Key** thành **AwsAccountId**. Đặt **Operator** thành **EQUALS**. Đặt **Value** thành ID tài khoản của bạn. ID tài khoản có thể được tìm thấy ở góc trên bên phải của bảng điều khiển AWS. Đảm bảo không có khoảng trắng hoặc dấu gạch ngang khi sao chép ID tài khoản.
![VPC](/images/4/4.1/s19.png)

20.   Chọn **Refresh** để xem trước các phát hiện đáp ứng tiêu chí.
![VPC](/images/4/4.1/s20.png)

{{%notice warning%}}
**Lưu ý cho độc giả**:  Do hạn chế trong Workshop Studio, bạn chỉ có một tài khoản, vì vậy quy tắc tự động hóa này sẽ áp dụng cho tất cả các phát hiện của bạn. Tuy nhiên, trong kiến trúc nhiều tài khoản mà bạn đã thiết lập quản trị phân quyền qua AWS Organization, bạn sẽ có thể làm theo hướng dẫn tương tự và xác định một hoặc nhiều tài khoản, chọn lọc, là môi trường sản xuất.
{{%/notice%}}

21. Cuộn xuống phần **Automated** action. Đối với **Note** nhập "This finding is in a production account (identified via automation rule)".



22. Chọn **Add another key-value pair**. Đối với **Key** nhập "Environment". Đối với **Value** nhập "Production".
![VPC](/images/4/4.1/s22.png)


23. Giữ các cài đặt mặc định khác và chọn **Create rule**.



24. Nếu bạn muốn xem lại một rule bạn đã tạo, bạn có thể chọn **Name** của rule từ trang Automations . Chọn tên **Tag Production findings**.

![VPC](/images/4/4.1/s24b.png)
Một rule khác:
![VPC](/images/4/4.1/s24a.png)

#### Tự động thêm các trường do người dùng định nghĩa vào các phát hiện phù hợp với mục tiêu của tổ chức
Tiếp theo, thiết lập tự động hóa tương tự như rule tự động hóa trước đó, nhưng thay vì thêm các trường do người dùng định nghĩa vào các phát hiện trong các tài khoản sản xuất, thêm chúng vào các phát hiện đáp ứng tiêu chí của mục tiêu của tổ chức. Trong trường hợp này, mục tiêu của tổ chức sẽ là bảo mật các AWS và IAM cccount. Để thực hiện điều này, tiêu chí tự động hóa của bạn sẽ là các phát hiện từ các kiểm soát Security Hub cho"Account" và "IAM". 

35. (Mục 35 là từ Workshop, không phải lỗi từ tác giả) Quay lại trang **Automations** trong Security Hub.


36. Chọn **Create rule**.


37. Chọn **Create custom rule**.



38. Thay đổi **Rule name** thành "Tag findings for security goal".



39. Thay đổi **Rule description** thành "Tag findings for security goal, secure IAM and accounts.".


40. Chúng ta muốn tiêu chí của chúng ta phù hợp với tất cả các phát hiện cho các kiểm soát Security Hub về "Account" hoặc "IAM". Dưới Criteria, chọn GeneratorId cho Key.


41. Chọn **PREFIX** cho **Operator**. Bằng cách chọn "PREFIX" chúng ta không cần tiêu chí cho từng kiểm soát riêng lẻ. Nếu bạn đã bật "Consolidated control findings" (đây là trường hợp cho tài khoản này), bạn chỉ cần tìm 2 tiền tố ID kiểm soát.


42. Đối với **Value**, nhập "security-control/Account". Điều này sẽ phù hợp với bất kỳ phát hiện nào cho ID kiểm soát Security Hub bắt đầu bằng "security-control/Account".



43. Dưới ô nhập **Value**, chọn **Add another value**. Điều này sẽ tạo một Operator và Value thứ hai cho cùng một Key.


44. Đối với **Operator** và **Value** thứ hai, nhập **PREFIX** và "security-control/IAM", tương ứng.
![VPC](/images/4/4.1/s44.png)

45. Chọn Refresh để xem trước các phát hiện phù hợp.

![VPC](/images/4/4.1/s45.png)

46. Dưới Automated action, chọn **Add another key-value pair**. Đối với Key, nhập "security-goal". Đối với **Value**, nhập "account-and-iam".


47. Giữ các cài đặt mặc định khác và chọn **Create rule**. Nhớ rằng, bạn có thể sử dụng các trường do người dùng định nghĩa để lọc các phát hiện và tạo ra các thông tin chi tiết trong Security Hub nếu bạn muốn theo dõi mục tiêu dễ dàng hơn ở cấp độ tổ chức và tài khoản.
![VPC](/images/4/4.1/s47.png)
#### Xem xét các phát hiện được cập nhật bởi các quy tắc tự động hóa

48. Điều hướng đến trang **Findings** trong Security Hub.


49. Tìm một phát hiện có biểu tượng ghi chú bên cạnh thời gian Updated.


50. Nhấn tiêu đề của một phát hiện có biểu tượng ghi chú bên cạnh thời gian Updated để mở chi tiết phát hiện.
![VPC](/images/4/4.1/s50.png)

51.  Mở rộng phần **Notes** để xem ghi chú được tạo bởi quy tắc tự động hóa.


52. Chọn tab **History** ở đầu chi tiết phát hiện. Bạn có thể xem dấu thời gian về thời điểm và cách phát hiện được cập nhật bởi quy tắc tự động hóa.
![VPC](/images/4/4.1/s52.png)

Xem một phát hiện khác (sau 5 phút):
![VPC](/images/4/4.1/s52b.png)

Xem toàn bộ lịch sử:
![VPC](/images/4/4.1/s52d.png)

Sau 3 phút nữa, ghi chú được cập nhật:
![VPC](/images/4/4.1/s52c.png)