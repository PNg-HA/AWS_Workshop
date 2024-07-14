---
title : "Connect to Private instance"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2. </b> "
---
For **Windows instance** located in **private subnet**, there is no **public IP**, no **internet gateway** so it cannot go out **internet.**\
With this type of instance, the traditional way is to use Bastion host technique which is expensive and laborious, but here we will use Session Manager with this type.\
Basically, the **private instance** still has to open the **TCP 443** port to **System Manager**, but we don't want to allow connection go out to the internet, but only in its  VPC, to enhance our security posture.\
To do that, we have to include the System Manager endpoint in the VPC, that is, using the **VPC interface endpoint:**

![ConnectPrivate](/images/arc-03.png) 

**VPC interface endpoint** is attached to the subnet, so this method can be done not only with **private subnet** but also with **public subnet**, meaning that with **public subnet**, you can completely prohibit **TCP 443** go out to the internet.

### Content:
   - [Enable DNS hostnames](./3.2.1-enablevpcdns/)
   - [Create VPC Endpoint](./3.2.2-createvpcendpoint/)
   - [Connect Private Instance](./3.3.3-connectec2/)