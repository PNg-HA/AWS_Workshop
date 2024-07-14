---
title : "Tạo Public Linux EC2"
date :  "`r Sys.Date()`" 
weight : 5
chapter : false
pre : " <b> 2.1.5 </b> "
---

1. Truy cập [giao diện quản trị dịch vụ EC2](https://console.aws.amazon.com/ec2/v2/home)
  + Click **Instances**.
  + Click **Launch instances**.
  
![EC2](/images/2.prerequisite/027-createec2.png)

2. Tại trang **Step 1: Choose an Amazon Machine Image (AMI)**.
  + Click **Select** để lựa chọn AMI **Amazon Linux 2 AMI**.
  
![EC2](/images/2.prerequisite/028-createec2.png)

3. Tại trang **Step 2: Choose an Instance Type**.
 + Click chọn Instance type **t2.micro**.
 + Click **Next: Configure Instance Details**.
 
![EC2](/images/2.prerequisite/029-createec2.png)

4. Tại trang **Step 3: Configure Instance Details**
  + Tại mục **Network** chọn **Lab VPC**.
  + Tại mục **Subnet** chọn **Lab Public Subnet**.
  + Tại mục **Auto-assign Public IP** chọn **Use subnet setting (Enable)**
  + Click **Next: Add Storage**.

![EC2](/images/2.prerequisite/030-createec2.png)

5. Click **Next: Add Tags** để chuyển sang bước kế tiếp.
  + Click **Next: Configure Security Group** để chuyển sang bước kế tiếp.


6. Tại trang **Step 6: Configure Security Group**.
  + Chọn **Select an existing security group**.
  + Chọn security group **SG Public Linux Instance**.
  + Click **Review and Launch**.

![EC2](/images/2.prerequisite/031-createec2.png)

7. Hộp thoại cảnh báo hiện lên vì chúng ta không cấu hình tường lửa cho phép kết nối vào port 22, Click **Continue** để tiếp tục.

8. Tại trang **Step 7: Review Instance Launch**.
  + Click **Launch**.

9. Tại hộp thoại **Select an existing key pair or create a new key pair**.
  + Click chọn **Create a new key pair**.
  + Tại mục **Key pair name** điền **LabKeypair**.
  + Click **Download Key Pair** và lưu xuống máy tính của bạn.
  + Click **Launch Instances** để tạo máy chủ EC2.

![EC2](/images/2.prerequisite/032-createec2.png)

10. Click **View Instances** để quay lại danh mục EC2 instances.

11. Click vào biểu tượng edit dưới cột **Name**.
  + Tại hộp thoại **Edit Name** điền **Public Linux Instance**.
  + Click **Save**.

![EC2](/images/2.prerequisite/033-createec2.png)

Tiếp theo chúng ta sẽ thực hiện tương tự để tạo 1 EC2 Instance Windows chạy trong Private subnet.