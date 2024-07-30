---
title : "Create S3 Gateway endpoint"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---


1. Go to [VPC service management console](https://console.aws.amazon.com/vpc/home)
  + Click **Endpoints**.
  + Click **Create endpoint**.

2. At the **Create endpoint** page.
  + In the **Name tag** field, enter **S3GW**.
  + In the **Service Category** section, click **AWS services**.
  + In the search box enter **S3**, then select **com.amazonaws.[region].s3**

![S3](/images/4.s3/008-s3.png)

3. In the **Services** section, select **com.amazonaws.[region].s3** with the Type of **Gateway**.
  + In the **VPC** section, select **Lab VPC**.
  + In the **Route tables** section, select both route tables.
  
![S3](/images/4.s3/009-s3.png)

4. Scroll down, click **Create endpoint**.

The next step is to configure Session Manager to store session logs to the S3 bucket we created.