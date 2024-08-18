---
title : "Detective - Tóm tắt"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.4.2 </b> "
---
Sử dụng trang Summary trong Amazon Detective để xác định các thực thể cần điều tra nguồn gốc của hoạt động trong 24 giờ trước đó. Trang Amazon Detective Summary giúp bạn xác định các thực thể liên quan đến các loại hoạt động bất thường cụ thể. Đây là một trong số nhiều điểm khởi đầu có thể có cho một cuộc điều tra.

Để hiển thị trang Summary, trong bảng điều hướng của Detective, chọn Summary. Trang Summary cũng được hiển thị mặc định khi bạn lần đầu mở bảng điều khiển của Detective.

Từ trang Summary bạn có thể xác định các thực thể đáp ứng các tiêu chí sau:
- Các cuộc điều tra cho thấy các sự kiện bảo mật tiềm ẩn được Detective xác định.
- Các thực thể tham gia vào hoạt động diễn ra ở các vị trí địa lý mới được quan sát.
- Các thực thể thực hiện số lượng gọi API lớn nhất.
- Các EC2 instance có lưu lượng truy cập lớn nhất.
- Các cụm container có số lượng container lớn nhất.

Từ mỗi bảng trang Summary, bạn có thể chuyển đến một thực thể được chọn.

Khi bạn xem xét trang Summary, bạn có thể điều chỉnh Phạm vi thời gian để xem hoạt động trong bất kỳ khung thời gian 24 giờ nào trong 365 ngày trước đó. Khi bạn thay đổi Ngày và giờ Bắt đầu, Ngày và giờ Kết thúc sẽ tự động được cập nhật thành 24 giờ sau thời gian bắt đầu bạn đã chọn.

Với Detective, bạn có thể truy cập dữ liệu sự kiện lịch sử lên đến một năm. Dữ liệu này có sẵn thông qua một tập hợp trực quan hóa hiển thị các thay đổi về loại và khối lượng hoạt động trong khoảng thời gian đã chọn. Detective liên kết những thay đổi này với những phát hiện của GuardDuty.