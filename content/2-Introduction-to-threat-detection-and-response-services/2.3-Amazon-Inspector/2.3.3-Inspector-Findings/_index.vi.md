---
title : "Inspector  - Phát hiện sai phạm"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3.3 </b> "
---

#### Lọc các phát hiện sai phạm

1. Điều hướng đến **Findings: All Findings** (https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/findings/all ).


2. Nhấp vào ô nhập ở đầu trang được gắn nhãn **Filter criteria** với nội dung "Add filter". Bộ lọc phát hiện sai phạm cho phép bạn chỉ xem các phát hiện sai phạm khớp với tiêu chí bạn chỉ định. Các phát hiện sai phạm không khớp với tiêu chí bộ lọc sẽ bị loại. Hãy dành một chút thời gian để xem qua tất cả các tùy chọn lọc danh sách phát hiện sai phạm của bạn.



3. Bạn có thể tạo bộ lọc trên mỗi tab để tập trung vào các loại phát hiện sai phạm cụ thể. Thử lọc theo resource tag. Nhấp vào trường **Add filter**. Chọn **Resource tag**. Lưu ý rằng các bộ lọc có phân biệt chữ hoa chữ thường. Nhập **"Environment"** cho Key. Nhập "**Development**" cho **Value**. Nhấp vào **Apply**. Điều này sẽ hiển thị các phát hiện sai phạm cho các resources có tag được áp dụng.


4. Bây giờ hãy xóa bộ lọc mà bạn vừa thêm bằng cách nhấp vào **Clear filters**.



5. Thêm một bộ lọc mới. Nhấp vào trường **Add filter**. Chọn **Resource tag**. Lưu ý rằng các bộ lọc có phân biệt chữ hoa chữ thường. Nhập "**Name**" cho Key. Nhập "**EC2InstanceDev3**" cho **Value**. Điều này sẽ chỉ hiển thị các phát hiện sai phạm cho các EC2 instance có tên "EC2InstanceDev3". Bạn có thể tùy chọn thử thêm nhiều bộ lọc cùng một lúc để thu hẹp tìm kiếm của mình.


6. Bây giờ hãy xóa bộ lọc mà bạn vừa thêm bằng cách nhấp vào **Clear** filters.


#### Xem xét một phát hiện sai phạm về khả năng truy cập mạng cho EC2

7. Nhấp vào trường **Add filter**. Chọn **Type** sau đó chọn **Network Reachability**. Nhấp vào **Apply**.


8. Nhấp vào tiêu đề của một trong các phát hiện sai phạm đã lọc để xem báo cáo phát hiện sai phạm. Trong báo cáo phát hiện sai phạm, bạn có thể thấy thông tin như Severity, Open port range, Account ID, hông tin về tài nguyên, và Open Network Paths.


#### Xem xét một phát hiện sai phạm về lỗ hổng code cho Lambda
9. Loại bỏ tất cả các bộ lọc bằng cách nhấp vào **Clear filters**. Nhấp vào trường **Add filter**. Chọn **Type** sau đó chọn **Code Vulnerability**. Nhấp vào **Apply**.


10. Nhấp vào tiêu đề của phát hiện sai phạm, **CWE-117,93 - Log injection**, để xem báo cáo phát hiện sai phạm.



11. phát hiện sai phạm nêu rằng "User-provided inputs must be sanitized before they are logged. An attacker can use unsensitized input to break a log's integrity, forge log entries, or bypass log monitors." Các chi tiết trong báo cáo bao gồm thông tin về tài nguyên và gợi ý khắc phục. Đọc cách khắc phục được đề xuất. Để thử thách, hãy quay lại sau nếu bạn có thời gian và cố gắng khắc phục sự cố mà KHÔNG xóa chức năng.


12. Đóng báo cáo phát hiện sai phạm và loại bỏ tất cả các bộ lọc bằng cách nhấp vào **Clear filters** một lần nữa.