---
title : "Create Endpoint ec2messages"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.2.2.3 </b> "
---


#### Create VPC Endpoint EC2MESSAGES

1. Go to [VPC service management console](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.
  
2. At the **Create endpoint** page.
  + In the **Name tag** field, enter **SSMMESSAGES**.
  + In the **Service Category** section, select **AWS Services**.
  + In the **Service Name** section,
  + In the **Service category** select: **AWS services**
  + In the field **Service Name** enter: **ec2** then select **Service Name: com.amazonaws.ap-southeast-1.ec2messages**.

![Connect](/images/3.connect/015-connect.png)

3. In the **Service Name** column, click **com.amazonaws.ap-southeast-1.ec2messages**.
  + In the **VPC** section, select **Lab VPC**.
  + Select the first AZ, then select the **Lab Private Subnet** subnet.
  
![Connect](/images/3.connect/016-connect.png)

4. Scroll down.
  + In the **Security Group** section, select the Security group **SG VPC Endpoint** that we created earlier.
  + In the **Policy** section, select **Full access**

5. Scroll down.
  + Click **Create endpoint**.

6. We have created the VPC Interface Endpoint **EC2MESSAGES**.

7. Make sure the 3 required endpoints have been created as shown below.

![Connect](/images/3.connect/018-connect.png)