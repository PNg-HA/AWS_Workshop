---
title : "Port Forwarding"
date :  "`r Sys.Date()`" 
weight : 5 
chapter : false
pre : " <b> 5. </b> "
---

{{% notice info %}}
**Port Forwarding** is a useful way to redirect network traffic from one IP address - Port to another IP address - Port. With **Port Forwarding** we can access an EC2 instance located in the private subnet from our workstation.
{{% /notice %}}

We will configure **Port Forwarding** for the RDP connection between our machine and **Private Windows Instance** located in the private subnet we created for this exercise.

![port-fwd](/images/arc-04.png) 

#### Create IAM user with permission to connect SSM

1. Go to [IAM service management console](https://console.aws.amazon.com/iamv2/home)
   + Click **Users** , then click **Add users**.

![FWD](/images/5.fwd/001-fwd.png)

2. At the **Add user** page.
   + In the **User name** field, enter **Portfwd**.
   + Click on **Access key - Programmatic access**.
   + Click **Next: Permissions**.
  
![FWD](/images/5.fwd/002-fwd.png)

3. Click **Attach existing policies directly**.
   + In the search box, enter **ssm**.
   + Click on **AmazonSSMFullAccess**.
   + Click **Next: Tags**, click **Next: Reviews**.
   + Click **Create user**.

4. Save **Access key ID** and **Secret access key** information to perform AWS CLI configuration.

#### Install and Configure AWS CLI and Session Manager Plugin
  
To perform this hands-on, make sure your workstation has [AWS CLI]() and [Session Manager Plugin](https://docs.aws.amazon.com/systems-manager/latest/userguide/session) installed -manager-working-with-install-plugin.html)

More hands-on tutorials on installing and configuring the AWS CLI can be found [here](https://000011.awsstudygroup.com/).

{{%notice tip%}}
With Windows, when extracting the **Session Manager Plugin** installation folder, run the **install.bat** file with Administrator permission to perform the installation.
{{%/notice%}}

#### Implement Portforwarding

1. Run the command below in **Command Prompt** on your machine to configure **Port Forwarding**.

```
   aws ssm start-session --target (your ID windows instance) --document-name AWS-StartPortForwardingSession --parameters portNumber="3389",localPortNumber="9999" --region (your region)
```
{{%notice tip%}}

**Windows Private Instance** **Instance ID** information can be found when you view the EC2 Windows Private Instance server details.

{{%/notice%}}

   + Example command:

```
C:\Windows\system32>aws ssm start-session --target i-06343d7377486760c --document-name AWS-StartPortForwardingSession --parameters portNumber="3389",localPortNumber="9999" --region ap-southeast-1
```

{{%notice warning%}}

If your command gives an error like below: \
SessionManagerPlugin is not found. Please refer to SessionManager Documentation here: http://docs.aws.amazon.com/console/systems-manager/session-manager-plugin-not-found\
Prove that you have not successfully installed the Session Manager Plugin. You may need to relaunch **Command Prompt** after installing **Session Manager Plugin**.

{{%/notice%}}

2. Connect to the **Private Windows Instance** you created using the **Remote Desktop** tool on your workstation.
   + In the Computer section: enter **localhost:9999**.


![FWD](/images/5.fwd/003-fwd.png)


3. Return to the administration interface of the System Manager - Session Manager service.
   + Click tab **Session history**.
   + We will see session logs with Document name **AWS-StartPortForwardingSession**.


![FWD](/images/5.fwd/004-fwd.png)


Congratulations on completing the lab on how to use Session Manager to connect and store session logs in S3 bucket. Remember to perform resource cleanup to avoid unintended costs.