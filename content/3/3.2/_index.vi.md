---
title : "Tổng hợp phát hiện từ nhiều tài khoản AWS"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2. </b> "
---
Khi bạn sử dụng cả Security Hub và AWS Organizations cùng nhau, bạn có thể tự động kích hoạt Security Hub cho tất cả các tài khoản của bạn, bao gồm cả các tài khoản mới khi chúng được thêm vào. Điều này làm tăng vi kiểm tra và phát hiện của Security Hub cung cấp cái nhìn tổng quan và chính xác hơn về tình trạng bảo mật tổng thể của bạn. Bằng cách sử dụng tính năng quản trị viên ủy quyền của Security Hub cùng với cấu hình trung tâm, bạn đạt được cái nhìn tập trung về bảo mật của bạn trên AWS qua các dịch vụ AWS và dịch vụ đối tác, trên các tài khoản và các regions.

**Lưu ý cho độc giả**: Mặc dù workshop nói rằng "At this time, we are unable to demonstrate multi-account aggregation in this workshop". Tuy nhiên, trong phần **Automated Security Response on AWS** sơ đồ kiến trúc cho thấy rằng các stack CloudFormation đã được triển khai ở nhiều tài khoản AWS.