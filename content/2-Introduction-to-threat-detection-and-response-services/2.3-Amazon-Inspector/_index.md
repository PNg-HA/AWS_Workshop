---
title : "Create IAM Role"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2 </b> "
---

### Create IAM Role

In this step, we will proceed to create IAM Role. In this IAM Role, the policy **AmazonSSMManagedInstanceCore** will be assigned, this is the policy that allows the EC2 server to communicate with the Session Manager.

1. Go to [IAM service administration interface](https://console.aws.amazon.com/iamv2/)
2. In the left navigation bar, click **Roles**.

![role](/images/2.prerequisite/038-iamrole.png)

3. Click **Create role**.

![role1](/images/2.prerequisite/039-iamrole.png)

4. Click **AWS service** and click **EC2**.
  + Click **Next: Permissions**.

![role1](/images/2.prerequisite/40-iamrole.png)

5. In the Search box, enter **AmazonSSMManagedInstanceCore** and press Enter to search for this policy.
  + Click the policy **AmazonSSMManagedInstanceCore**.
  + Click **Next: Tags.**

![createpolicy](/images/2.prerequisite/041-iamrole.png)

6. Click **Next: Review**.
7. Name the Role **SSM-Role** in Role Name
  + Click **Create Role** \.

![namerole](/images/2.prerequisite/042-iamrole.png)

Next, we will make the connection to the EC2 servers we created with **Session Manager**.