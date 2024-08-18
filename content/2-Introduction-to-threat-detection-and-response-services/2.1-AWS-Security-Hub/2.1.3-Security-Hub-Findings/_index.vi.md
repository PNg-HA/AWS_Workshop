---
title : "Security Hub - Phát hiện sai phạm"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

#### Phát hiện sai phạm trong Security Hub

1. Nhấp vào **Findings** từ bảng điều hướng bên trái trong Security Hub.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s1.png)
2. Bạn có thể sử dụng bộ lọc để thu hẹp danh sách các Phát hiện sai phạm được hiển thị. Nhấp vào **Add filter**.

3. Chọn **Product name** và sau đó nhập "GuardDuty" (chú ý viết hoa). Nhấp vào **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s3.png)
4. Điều này sẽ hiển thị tất cả các Phát hiện sai phạm mà Security Hub đã nhận được từ dịch vụ Phát hiện sai phạm mối đe dọa, GuardDuty. Có nhiều Phát hiện sai phạm của Security Hub được liệt kê ở đây.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s4.png)
Hãy thử thêm bộ lọc để thu hẹp danh sách xuống chỉ còn các Phát hiện sai phạm nghiêm trọng. Nhấp vào **Add filters** một lần nữa.

5. Từ danh sách thả xuống, chọn **Severity label** và nhập **HIGH**. Chú ý viết hoa. Nhấp vào **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s5a.png)
Kết quả:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s5b.png)
6. Chọn một trong các Phát hiện sai phạm và nhấp vào tiêu đề. Điều này sẽ mở bảng chi tiết Phát hiện sai phạm. Mở rộng tất cả các phần và dành thời gian để xem thông tin ở đây. Bạn có thể thấy mô tả, liên kết đến hướng dẫn khắc phục, thông tin về tài nguyên và nhiều hơn nữa.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s6.png)
7. Trong bảng chi tiết Phát hiện sai phạm, nhấp vào ID Phát hiện sai phạm ở đầu bảng để hiển thị JSON đầy đủ của Phát hiện sai phạm. JSON của Phát hiện sai phạm có thể được tải xuống nếu cần thiết cho việc điều tra thêm.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s7.png)
8. Đóng cửa sổ pop-up JSON bằng cách nhấp vào **X** ở góc trên bên phải.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s8.png)
9. Ở đầu chi tiết Phát hiện sai phạm, mở tab **History** để xem danh sách theo trình tự thời gian của tất cả các thay đổi đã được thực hiện đối với Phát hiện sai phạm. Độ minh bạch của lịch sử Phát hiện sai phạm giúp bạn xác định các rủi ro bảo mật tiềm ẩn nhanh hơn và thực hiện các bước chủ động để giảm thiểu chúng.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s9.png)
10. Đóng bảng chi tiết Phát hiện sai phạm bằng cách nhấp vào X ở góc trên bên phải, nhưng vẫn ở trang **Findings**.

11. Hãy cố gắng hiểu các tài nguyên trong môi trường của chúng ta đang tạo ra nhiều Phát hiện sai phạm nhất. Loại bỏ tất cả các bộ lọc. Sau đó, thêm một bộ lọc mới, chọn **Resource type** và chọn **not** sau đó nhập **AwsAccount**. Nhấp vào **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s11.png)
12.  Thêm một bộ lọc khác. Chọn **Record state**, sau đó nhập **ACTIVE**. Nhấp vào **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s12.png)
13.  Nhấp vào **Add filters** trong thanh tìm kiếm một lần nữa. Lần này, chọn **Group by** và chọn **ResourceId**. Nhấp vào **Apply**. Danh sách này cho bạn thấy số lượng Phát hiện sai phạm theo từng tài nguyên.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s13.png)
14.  Xem chi tiết bằng cách nhấp vào **Insight details**:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s14.png)
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s15.png)

15.  Xem các Phát hiện sai phạm khác được lọc theo loại nhà cung cấp **TTPs/Discovery/Recon:IAMUser-MaliciousIPCallerCustom**, được tạo ra bởi **GuardDuty**
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o1.png)
16.  Nhấp vào một Phát hiện sai phạm để xem chi tiết hơn.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o2.png)
Nó cho biết rằng một API ListBuckets đã được gọi bởi các tác nhân không được ủy quyền, được nhận diện từ IP trên danh sách mối đe dọa tùy chỉnh của bucket.
17.  Xem thêm các Phát hiện sai phạm được tạo ra bởi **Inspector**
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o3.png)
Những Phát hiện sai phạm này được tạo ra khi **Inspector** Phát hiện sai phạm các lỗ hổng bằng cách quét các EC2 instance .

