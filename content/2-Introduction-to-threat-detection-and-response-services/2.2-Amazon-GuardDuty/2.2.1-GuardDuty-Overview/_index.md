---
title : "GuardDuty - Overview"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.2.1 </b> "
---


#### GuardDuty - Overview
Amazon GuardDuty is a threat detection service that continuously monitors for malicious activity and unauthorized behavior across your AWS environment. GuardDuty combines machine learning (ML), anomaly detection, and malicious file discovery, using both AWS and industry-leading third-party sources to help protect your AWS accounts, workloads, and data. GuardDuty is capable of analyzing tens of billions of events across multiple AWS data sources, including AWS CloudTrail logs, Amazon Virtual Private Cloud (Amazon VPC) Flow Logs, and DNS query logs. GuardDuty also monitors Amazon Simple Storage Service (Amazon S3) data events, Amazon Aurora login events, and runtime activity for Amazon Elastic Kubernetes Service (Amazon EKS), Amazon Elastic Compute Cloud (Amazon EC2), and Amazon Elastic Container Service (Amazon ECS)—including serverless container workloads on AWS Fargate.

GuardDuty gives you accurate threat detection of compromised accounts, which can be difficult to detect quickly if you are not continuously monitoring factors in near real time. GuardDuty can detect signs of account compromise, such as AWS resource access from an unusual geolocation at an atypical time of day. For programmatic AWS accounts, GuardDuty checks for unusual API calls, such as attempts to obscure account activity by disabling CloudTrail logging or taking snapshots of a database from a malicious IP address.

GuardDuty continuously monitors and analyzes your AWS account and workload event data found in CloudTrail, VPC Flow Logs, and DNS logs. There is no additional security software or infrastructure to deploy and maintain for the foundational protections in GuardDuty. By associating your AWS accounts together, you can aggregate threat detection instead of working on an account-by-account basis. In addition, you do not have to collect, analyze, and correlate large volumes of AWS data from multiple accounts. Focus on how to respond quickly, how to keep your organization secure, and continuing to scale and innovate on AWS.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.1-GuardDuty-Overview/s1.png)