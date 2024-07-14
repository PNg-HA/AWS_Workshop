---
title : "Update IAM Role"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

For our EC2 instances to be able to send session logs to the S3 bucket, we will need to update the IAM Role assigned to the EC2 instance by adding a policy that allows access to S3.

#### Update IAM Role

1. Go to [IAM service management console](https://console.aws.amazon.com/iamv2/home?#/home)
  + Click **Roles**.
  + In the search box, enter **SSM**.
  + Click on the **SSM-Role** role.

![S3](/images/4.s3/002-s3.png)

2. Click **Attach policies**.
 
![S3](/images/4.s3/003-s3.png)

3. In the Search box enter **S3**.
  + Click the policy **AmazonS3FullAccess**.
  + Click **Attach policy**.
 
![S3](/images/4.s3/004-s3.png)
 
{{%notice tip%}}
In the production environment, we will grant stricter permissions to the specified S3 bucket. In the framework of this lab, we use the policy **AmazonS3FullAccess** for convenience.
{{%/notice%}}

Next, we will proceed to create an S3 bucket to store session logs.