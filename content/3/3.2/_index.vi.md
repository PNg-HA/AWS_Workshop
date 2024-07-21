---
title : "Tạo kết nối đến máy chủ EC2 Private"
date :  "`r Sys.Date()`" 
weight : 2 
chapter : false
pre : " <b> 3.2. </b> "
---
Đối với **Windows instance** nằm trong **private subnet**, không có **public IP**, không có **internet gateway** nên không thể đi ra ngoài **internet.**\
Với loại instance này, cách làm truyền thống là ta sẽ sử dụng kỹ thuật Bastion host tốn nhiều chi phí và công sức, nhưng ở đây chúng ta sẽ sử dụng Session Manager với loại này.\
Cơ bản là **private instance** vẫn phải mở cổng **TCP 443** tới **System Manager**, nhưng không cho kết nối đó đi ra ngoài internet mà chỉ cho đi trong chính VPC của mình, nên đảm bảo được vấn đề bảo mật.\
Để làm được điều đó, ta phải đưa endpoint của System Manager vào trong VPC, nghĩa là sử dụng **VPC interface endpoint:** 

![ConnectPrivate](/images/arc-03.png) 

**VPC interface endpoint** được gắn với subnet nên cách làm này không những với **private subnet** mà còn có thể làm với **public subnet**, nghĩa là với **public subnet**, bạn hoàn toàn có thể không cho **TCP 443** đi ra ngoài internet.

### Nội dung:
   - [Kích hoạt DNS hostnames](./3.2.1-enablevpcdns/)
   - [Tạo VPC Endpoint](./3.2.2-createvpcendpoint/)
   - [Kết nối Private Instance](./3.3.3-connectec2/)
