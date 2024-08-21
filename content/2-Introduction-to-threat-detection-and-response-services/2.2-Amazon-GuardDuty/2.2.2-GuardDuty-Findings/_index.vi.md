---
title : "GuardDuty - Phát hiện bảo mật"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2.2 </b> "
---

Một phát hiện của GuardDuty đại diện cho một vấn đề bảo mật tiềm ẩn được phát hiện trong mạng của bạn. GuardDuty tạo ra một phát hiện bất cứ khi nào nó phát hiện hoạt động bất thường và có khả năng độc hại trong môi trường AWS của bạn. Bạn có thể xem và quản lý các phát hiện của GuardDuty trên trang Findings trong bảng điều khiển GuardDuty hoặc bằng cách sử dụng các thao tác CLI hoặc API của GuardDuty.

#### Xem xét một phát hiện trong GuarDuty
1. Điều hướng đến **Findings** trong Amazon GuardDuty bằng cách chọn **Findings** trong bảng điều hướng bên trái.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s1.png)
Dưới đây là các phát hiện trong tài khoản AWS Workshop:
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s1b.png)

2. Chọn một trong các phát hiện trên trang bằng cách chọn hàng. Điều này sẽ mở tóm tắt phát hiện ở phía bên phải của trang. Chi tiết phát hiện thay đổi dựa trên loại phát hiện.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s2.png)

Kiểm tra một phát hiện khác trong workshop:
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s2b.png)

3. Xem xét phát hiện mà bạn đã mở.


#### Hiểu về mức độ nghiêm trọng của phát hiện trong GuardDuty

4. Tìm **Severity** của phát hiện mà bạn đã chọn.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s4.png)


5. Nếu bạn muốn xem hoặc tải xuống phát hiện dưới dạng JSON, bạn có thể chọn **Finding ID** ở đầu bản tóm tắt phát hiện.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s5a.png)

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s5b.png)


6. Chọn **X** ở góc trên bên phải của bản tóm tắt phát hiện để đóng nó.

#### Tìm và lọc các phát hiện bảo mật của GuardDuty

7. Chọn thanh Tìm kiếm nơi có ghi **Add filter criteria**.



8. Nhập **Severity** vào thanh tìm kiếm và chọn **Severity**, sau đó mở ra một menu phụ với các tùy chọn Low, Medium, và High.


9. Đánh dấu vào ô **High** và nhấp **Apply**. TĐiều này sẽ cập nhật danh sách các phát hiện hiển thị tương ứng.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s9a.png)

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s9b.png)

#### Quản lý các phát hiện bảo mật của GuardDuty
10. Với một phát hiện được chọn, chọn menu thả xuống **Actions** (góc trên bên phải của trang). Nhấp **"Archive"** để lưu trữ phát hiện.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s10.png)

11. Lưu trữ phát hiện sẽ ẩn nó khỏi danh sách các phát hiện hiện tại. Để xem nó, chọn menu thả xuống **"Current"**, và chọn **"Archived"** để xem phát hiện mà bạn vừa lưu trữ.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s11a.png)

Kết quả:
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s11b.png)

12.  Để hủy lưu trữ phát hiện, chọn nó, và sau đó nhấp lại vào menu thả xuống **Actions** (góc trên bên phải của trang). Lần này, nhấp **"Unarchive"** để hủy lưu trữ phát hiện.


![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s2.png)