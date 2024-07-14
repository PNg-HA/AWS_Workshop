---
title : "Manage session logs"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 4. </b> "
---


With Session Manager, we can view the history of connections to instances through **Session history**. However, we have not seen the details of the commands used in a session.

![S3](/images/4.s3/001-s3.png)

In this section, we will proceed to create an S3 bucket and configure the session logs feature to see the details of the commands used in the session.

![port-fwd](/images/arc-log.png) 

### Content:

   - [Update IAM Role](./4.1-updateiamrole/)
   - [Create **S3 Bucket**](./4.2-creates3bucket/)
   - [Create S3 Gateway endpoint](./4.3-creategwes3)
   - [Configure **Session logs**](./4.4-configsessionlogs/)