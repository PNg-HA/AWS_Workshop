---
title : "Detective - Nhóm phát Hiện"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.4.5 </b> "
---
Nhóm phát hiện sai phạm của Amazon Detective cho phép bạn kiểm tra nhiều hoạt động liên quan đến một sự kiện bảo mật tiềm ẩn. Bạn có thể phân tích nguyên nhân cốt lõi của các phát hiện sai phạm nghiêm trọng từ GuardDuty bằng cách sử dụng các nhóm phát hiện sai phạm. Nếu một tác nhân đe dọa đang cố gắng xâm nhập vào môi trường AWS của bạn, họ thường thực hiện một chuỗi hành động dẫn đến nhiều phát hiện sai phạm bảo mật và hành vi bất thường. Những hành động này thường được phân tán theo thời gian và các thực thể khác nhau. Khi các phát hiện sai phạm bảo mật được điều tra một cách riêng lẻ, có thể dẫn đến sự hiểu lầm về ý nghĩa của chúng và khó khăn trong việc tìm ra nguyên nhân cốt lõi. Amazon Detective giải quyết vấn đề này bằng cách áp dụng kỹ thuật phân tích đồ thị để suy luận mối quan hệ giữa các phát hiện sai phạm và các thực thể, và nhóm chúng lại với nhau. Chúng tôi khuyến nghị nên coi các nhóm phát hiện sai phạm là điểm khởi đầu để điều tra các thực thể và phát hiện sai phạm liên quan.

Detective phân tích dữ liệu từ các phát hiện sai phạm và nhóm chúng với các phát hiện sai phạm khác có khả năng liên quan dựa trên các tài nguyên mà chúng chia sẻ. Ví dụ, các phát hiện sai phạm liên quan đến các hành động thực hiện bởi cùng một phiên IAM role hoặc xuất phát từ cùng một địa chỉ IP rất có khả năng là một phần của cùng một hoạt động cơ bản. Việc điều tra các phát hiện sai phạm và bằng chứng theo nhóm là rất có giá trị, ngay cả khi các mối liên hệ được tạo ra bởi Detective không liên quan.

Ngoài các phát hiện sai phạm, mỗi nhóm còn bao gồm các thực thể liên quan đến các phát hiện sai phạm. Các thực thể có thể bao gồm các tài nguyên bên ngoài AWS như địa chỉ IP hoặc tác nhân người dùng.

Sau khi một phát hiện sai phạm GuardDuty ban đầu xảy ra và liên quan đến một phát hiện sai phạm khác, nhóm phát hiện sai phạm với tất cả các phát hiện sai phạm liên quan và tất cả các thực thể liên quan sẽ được tạo ra trong vòng 48 giờ.