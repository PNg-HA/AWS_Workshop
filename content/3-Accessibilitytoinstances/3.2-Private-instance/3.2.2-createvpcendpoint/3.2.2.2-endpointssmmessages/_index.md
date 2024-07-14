---
title : "Create Endpoint ssmmessages"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2.2.2 </b> "
---


#### Create VPC Endpoint SSMMESSAGES

1. Go to [VPC service management console](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.
  
2. At the **Create endpoint** page.
  + In the **Name tag** field, enter **SSMMESSAGES**.
  + In the **Service Category** section, select **AWS Services**.
  + In the **Service Name** section,
  + In the **Service category** select: **AWS services**
  + In the **Service Name** field enter: **ssmmessages** then select **Service Name: com.amazonaws.ap-southeast-1.ssmmessages**.

![Connect](/images/3.connect/012-connect.png)

3. In the **Service Name** column, click **com.amazonaws.ap-southeast-1.ssmmessages**.
  + In the **VPC** section, select **Lab VPC**.
  + Select the first AZ, then select the **Lab Private Subnet** subnet.
  
![Connect](/images/3.connect/013-connect.png)

4. Scroll down.
  + In the **Security Group** section, select the Security group **SG VPC Endpoint** that we created earlier.
  + In the **Policy** section, select **Full access**

5. Scroll down.
  + Click **Create endpoint**.

6. We have created the VPC Interface Endpoint **SSMMESSAGES**.