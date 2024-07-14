---
title : "Connect to Public Instance"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1. </b> "
---
![SSMPublicinstance](/images/arc-02.png)

1. Go to [EC2 service management console](https://console.aws.amazon.com/ec2/v2/home).
  + Click on **Public Linux Instance**.
  + Click **Actions**.
  + Click **Security**.
  + Click **Modify IAM role**.

![Connect](/images/3.connect/001-connect.png)

2. At the Modify IAM role page.
  + Click to select **SSM-Role**.
  + Click **Save**.

{{% notice note %}}
You will need to wait about 10 minutes before performing the next step. This time our EC2 instance will automatically register with the Session Manager.
{{% /notice %}}

3. Go to the [AWS Systems Manager service management console](https://console.aws.amazon.com/systems-manager/home)
  + Drag the left menu slider down.
  + Click **Session Manager**.
  + Click **Start Session**.


![Connect](/images/3.connect/002-connect.png)


4. Then select **Public Linux Instance** and click **Start session** to access the instance.

![Connect](/images/3.connect/003-connect.png)


5. Terminal will appear on the browser. Testing with the command ``` sudo tcpdump -nn port 22 ``` and ```sudo tcpdump ``` we will see no SSH traffic but only HTTPS traffic.

![Connect](/images/3.connect/004-connect.png)

{{% notice note %}}
 Above, we have created a connection to the public instance without opening SSH port 22, for better security, avoiding any attack to the SSH port.\
One disadvantage of the above method is that we have to open the Security Group outbound at port 443 to the internet. Since it's a public instance, it probably won't be a problem, but if you want extra security, you can block port 443 to the internet and still use the Session Manager. We will go through this in the private instance section below.
 {{% /notice %}}

 You can click terminate to end the currently connected session before proceeding to the next step.