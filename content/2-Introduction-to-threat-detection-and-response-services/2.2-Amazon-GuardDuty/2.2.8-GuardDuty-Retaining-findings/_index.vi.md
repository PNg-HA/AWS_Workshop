---
title : "GuardDuty - Giữ lại các phát hiện"
date : "`r Sys.Date()`"
weight : 8
chapter : false
pre : " <b> 2.2.8 </b> "
---


GuardDuty giữ lại các phát hiện được tạo ra trong một khoảng thời gian 90 ngày. GuardDuty xuất các phát hiện đang hoạt động đến Amazon EventBridge. Bạn có thể tùy chọn xuất các phát hiện được tạo ra đến một Amazon Simple Storage Service (Amazon S3) bucket. Điều này sẽ giúp bạn theo dõi dữ liệu lịch sử về các hoạt động đáng nghi trong tài khoản của bạn và đánh giá xem các bước khắc phục được khuyến nghị có thành công hay không.

Bất kỳ phát hiện hoạt động mới nào mà GuardDuty tạo ra đều được xuất tự động trong khoảng 5 phút sau khi phát hiện được tạo ra. Bạn có thể thiết lập tần suất cho việc cập nhật các phát hiện hoạt động được xuất đến EventBridge. Tần suất mà bạn chọn áp dụng cho việc xuất các trường hợp mới của các phát hiện hiện có đến EventBridge, S3 bucket của bạn (khi được cấu hình), và Detective (khi được tích hợp).

Khi bạn cấu hình các thiết lập để xuất phát hiện đến một Amazon S3 bucket, GuardDuty sử dụng AWS Key Management Service (AWS KMS) để mã hóa dữ liệu phát hiện trong bucket S3 của bạn. Điều này yêu cầu bạn phải thêm quyền vào bucket S3 của bạn và khóa AWS KMS để GuardDuty có thể sử dụng chúng để xuất phát hiện trong tài khoản của bạn.