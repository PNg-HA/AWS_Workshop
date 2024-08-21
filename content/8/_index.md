+++
title = "Clean up resources"
date = 2024
weight = 8
chapter = false
pre = "<b>8. </b>"
+++

This workshop is created in the AWS workshop template, so acctually you can just delete resource related to section 5.3. We will take the following steps to delete the resources we created in this exercise.

#### Delete Automated Security Response solution 

1. Remove the aws-sharr-member.template from each member account.

2. Remove the aws-sharr-admin.template from the admin account.
3. Remove SO0111-* IAM roles.
4. Remove CloudWatch logging.

#### Disable AWS Security Hub
1. Go to [General page of Security Hub](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/settings/general).
2. Click **Disable AWS Security Hub**. 
![VPC](/images/8/sh1.png)
3. There is a popup, click **Disable AWS Security Hub**. 
![VPC](/images/8/sh2.png)



#### Delete related services
1. Disable GuardDuty.
2. Disable Inspector.
3. Disable Detective.

