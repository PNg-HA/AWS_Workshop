---
title : "Security Hub - Phát hiện bảo mật"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

#### Phát hiện bảo mật trong Security Hub

1. Chọn **Findings** từ bảng điều hướng bên trái trong Security Hub.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s1.png)
2. Bạn có thể sử dụng bộ lọc để thu hẹp danh sách các phát hiện bảo mật được hiển thị. Chọn **Add filter**.

3. Chọn **Product name** và sau đó nhập "GuardDuty" (chú ý viết hoa). Chọn **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s3.png)
4. Điều này sẽ hiển thị tất cả các phát hiện bảo mật mà Security Hub đã nhận được từ dịch vụ phát hiện bảo mật mối đe dọa, GuardDuty. Có nhiều phát hiện bảo mật của Security Hub được liệt kê ở đây.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s4.png)
Hãy thử thêm bộ lọc để thu hẹp danh sách xuống chỉ còn các phát hiện bảo mật nghiêm trọng. Chọn **Add filters** một lần nữa.

5. Từ danh sách thả xuống, chọn **Severity label** và nhập **HIGH**. Chú ý viết hoa. Chọn **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s5a.png)
Kết quả:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s5b.png)
6. Chọn một trong các phát hiện bảo mật và chọn tiêu đề. Điều này sẽ mở bảng chi tiết phát hiện bảo mật. Mở rộng tất cả các phần và dành thời gian để xem thông tin ở đây. Bạn có thể thấy mô tả, liên kết đến hướng dẫn khắc phục, thông tin về tài nguyên và nhiều hơn nữa.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s6.png)
7. Trong bảng chi tiết phát hiện bảo mật, chọn ID phát hiện bảo mật ở đầu bảng để hiển thị JSON đầy đủ của phát hiện bảo mật. JSON của phát hiện bảo mật có thể được tải xuống nếu cần thiết cho việc điều tra thêm.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s7.png)
8. Đóng cửa sổ pop-up JSON bằng cách chọn **X** ở góc trên bên phải.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s8.png)
9. Ở đầu chi tiết phát hiện bảo mật, mở tab **History** để xem danh sách theo trình tự thời gian của tất cả các thay đổi đã được thực hiện đối với phát hiện bảo mật. Độ minh bạch của lịch sử phát hiện bảo mật giúp bạn xác định các rủi ro bảo mật tiềm ẩn nhanh hơn và thực hiện các bước chủ động để giảm thiểu chúng.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s9.png)
10. Đóng bảng chi tiết phát hiện bảo mật bằng cách chọn X ở góc trên bên phải, nhưng vẫn ở trang **Findings**.

11. Hãy cố gắng hiểu các tài nguyên trong môi trường của chúng ta đang tạo ra nhiều phát hiện bảo mật nhất. Loại bỏ tất cả các bộ lọc. Sau đó, thêm một bộ lọc mới, chọn **Resource type** và chọn **not** sau đó nhập **AwsAccount**. Chọn **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s11.png)
12.  Thêm một bộ lọc khác. Chọn **Record state**, sau đó nhập **ACTIVE**. Chọn **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s12.png)
13.  Chọn **Add filters** trong thanh tìm kiếm một lần nữa. Lần này, chọn **Group by** và chọn **ResourceId**. Chọn **Apply**. Danh sách này cho bạn thấy số lượng phát hiện bảo mật theo từng tài nguyên.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s13.png)
14.  Xem chi tiết bằng cách chọn **Insight details**:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s14.png)
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s15.png)

15.  Xem các phát hiện bảo mật khác được lọc theo loại bên cung cấp **TTPs/Discovery/Recon:IAMUser-MaliciousIPCallerCustom**, được tạo ra bởi **GuardDuty**
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o1.png)
16.  Chọn một phát hiện bảo mật để xem chi tiết hơn.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o2.png)
Nó cho biết rằng một API ListBuckets đã được gọi bởi các tác nhân không được ủy quyền, được nhận diện từ IP trên danh sách mối đe dọa tùy chỉnh của bucket.
17.  Xem thêm các phát hiện bảo mật được tạo ra bởi **Inspector**
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o3.png)
Những phát hiện bảo mật này được tạo ra khi **Inspector** phát hiện bảo mật các lỗ hổng bằng cách quét các EC2 instance .

