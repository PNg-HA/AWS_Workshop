---
title : "Create S3 Bucket"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---


In this step, we will create an S3 bucket to store session logs sent from EC2 instances.

#### Create **S3 Bucket**

1. Access [S3 service management console](https://s3.console.aws.amazon.com/s3/home)
  + Click **Create bucket**.

![S3](/images/4.s3/005-s3.png)

2. At the **Create bucket** page.
  + In the **Bucket name** field, enter the bucket name **lab-yourname-bucket-0001**
  + In the **Region** section, select **Region** you are doing the current lab.

![S3](/images/4.s3/006-s3.png)

 {{%notice tip%}}
The name of the S3 bucket must not be the same as all other S3 buckets in the system. You can substitute your name and enter a random number when generating the S3 bucket name.
{{%/notice%}}

3. Scroll down and click **Create bucket**.

![S3](/images/4.s3/007-s3.png)

 {{%notice tip%}}
When we created the S3 bucket we did **Block all public access** so our EC2 instances won't be able to connect to S3 via the internet.
In the next step, we will configure the S3 Gateway Endpoint feature to allow EC2 instances to connect to the S3 bucket via the VPC's internal network.
{{%/notice%}}