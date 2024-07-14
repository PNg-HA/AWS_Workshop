---
title : "Create VPC Endpoint"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2.2 </b> "
---


#### Create VPC Endpoint SSM

We will create 3 interface endpoints required by the Session Manager:
   - Interface endpoints:
     - com.amazonaws.region.ssm
     - com.amazonaws.region.ec2messages
     - com.amazonaws.region.ssmmessages

You can refer to more [here](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-prerequisites.html)

### Content:
  - [Create Endpoint ssm](./3.2.2.1-endpointssm/)
  - [Create Endpoint ssmmessages](./3.2.2.2-endpointssmmessages/)
  - [Create Endpoint ec2messages](./3.2.2.3-endpointec2messages/)