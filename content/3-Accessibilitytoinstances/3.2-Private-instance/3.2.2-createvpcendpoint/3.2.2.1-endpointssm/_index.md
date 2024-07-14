---
title : "Create Endpoint ssm"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.2.2.1 </b> "
---


#### Create VPC Endpoint SSM

1. Go to [VPC service management console](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.
  
![Connect](/images/3.connect/005-connect.png)

2. At the **Create endpoint** page.
  + In the **Name tag** field, enter **SSM**.
  + In the **Service Category** section, select **AWS Services**.
  + In the **Service Name** section,
  + In the **Service category** section, select: **AWS services**
  + In the **Service Name** section enter: **SSM** then select **Service Name: com.amazonaws.ap-southeast-1.ssm**.

![Connect](/images/3.connect/006-connect.png)

3. In the **Service Name** column, click **com.amazonaws.ap-southeast-1.ssm**.
  + In the **VPC** section, select **Lab VPC**.
  + Select the first AZ, then select the **Lab Private Subnet** subnet.
  
![Connect](/images/3.connect/007-connect.png)

4. Scroll down.
  + In the **Security Group** section, select the Security group **SG VPC Endpoint** that we created earlier.
  + In the **Policy** section, select **Full access**.

![Connect](/images/3.connect/008-connect.png)

5. Scroll down.
  + Click **Create endpoint**.

6. We have created the VPC Interface Endpoint for **SSM**.


![Connect](/images/3.connect/011-connect.png)