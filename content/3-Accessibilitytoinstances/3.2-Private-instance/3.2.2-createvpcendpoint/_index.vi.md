---
title : "Tạo VPC Endpoint"
date :  "`r Sys.Date()`" 
weight : 2
chapter : false
pre : " <b> 3.2.2 </b> "
---


#### Tạo VPC Endpoint SSM

- Chúng ta sẽ tạo 3 interface endpoint yêu cầu bởi Session Manager:
  - Interface endpoint:
    - com.amazonaws.region.ssm
    - com.amazonaws.region.ec2messages
    - com.amazonaws.region.ssmmessages

Bạn có thể tham khảo thêm [tại đây](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-prerequisites.html)

### Nội dung:
   - [Tạo Endpoint ssm](./3.2.2.1-endpointssm/)
   - [Tạo Endpoint ssmmessages](./3.2.2.2-endpointssmmessages/)
   - [Tạo Endpoint ec2messages](./3.2.2.3-endpointec2messages/)