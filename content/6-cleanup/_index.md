+++
title = "Clean up resources"
date = 2022
weight = 6
chapter = false
pre = "<b>6. </b>"
+++

We will take the following steps to delete the resources we created in this exercise.

#### Delete EC2 instance

1. Go to [EC2 service management console](https://console.aws.amazon.com/ec2/v2/home)
   + Click **Instances**.
   + Select both **Public Linux Instance** and **Private Windows Instance** instances.
   + Click **Instance state**.
   + Click **Terminate instance**, then click **Terminate** to confirm.

2. Go to [IAM service management console](https://console.aws.amazon.com/iamv2/home#/home)
   + Click **Roles**.
   + In the search box, enter **SSM**.
   + Click to select **SSM-Role**.
   + Click **Delete**, then enter the role name **SSM-Role** and click **Delete** to delete the role.

![Clean](/images/6.clean/001-clean.png)

3. Click **Users**.
   + Click on user **Portfwd**.
   + Click **Delete**, then enter the user name **Portfwd** and click **Delete** to delete the user.

#### Delete S3 bucket

1. Access [System Manager - Session Manager service management console](https://console.aws.amazon.com/systems-manager/session-manager).
   + Click the **Preferences** tab.
   + Click **Edit**.
   + Scroll down.
   + In the section **S3 logging**.
   + Uncheck **Enable** to disable logging.
   + Scroll down.
   + Click **Save**.

2. Go to [S3 service management console](https://s3.console.aws.amazon.com/s3/home)
   + Click on the S3 bucket we created for this lab. (Example: lab-fcj-bucket-0001 )
   + Click **Empty**.
   + Enter **permanently delete**, then click **Empty** to proceed to delete the object in the bucket.
   + Click **Exit**.

3. After deleting all objects in the bucket, click **Delete**

![Clean](/images/6.clean/002-clean.png)

4. Enter the name of the S3 bucket, then click **Delete bucket** to proceed with deleting the S3 bucket.

![Clean](/images/6.clean/003-clean.png)

#### Delete VPC Endpoints

1. Go to [VPC service management console](https://console.aws.amazon.com/vpc/home)
   + Click **Endpoints**.
   + Select the 4 endpoints we created for the lab including **SSM**, **SSMMESSAGES**, **EC2MESSAGES**, **S3GW**.
   + Click **Actions**.
   + Click **Delete VPC endpoints**.

![Clean](/images/6.clean/004-clean.png)

2. In the confirm box, enter **delete**.
   + Click **Delete** to proceed with deleting endpoints.

3. Click the refresh icon, check that all endpoints have been deleted before proceeding to the next step.

![Clean](/images/6.clean/005-clean.png)

#### Delete VPC

1. Go to [VPC service management console](https://console.aws.amazon.com/vpc/home)
   + Click **Your VPCs**.
   + Click on **Lab VPC**.
   + Click **Actions**.
   + Click **Delete VPC**.

2. In the confirm box, enter **delete** to confirm, click **Delete** to delete **Lab VPC** and related resources.

![Clean](/images/6.clean/006-clean.png)