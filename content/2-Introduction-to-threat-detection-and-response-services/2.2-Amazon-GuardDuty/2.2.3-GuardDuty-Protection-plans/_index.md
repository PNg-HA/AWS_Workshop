---
title : "GuardDuty - Protection plans"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.2.3 </b> "
---
#### Prequisite
Make sure the EC2 instance has the SSM Management Role.
#### Amazon S3 protection in Amazon GuardDuty

1. Visit the **S3 Protection** page under Protection plans in the GuardDuty console.


2. Make sure that S3 Protection is enabled.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s2.png)

#### EKS Protection in Amazon GuardDuty
3. Visit the **EKS Protection** page under Protection plans in the GuardDuty console.


4. Make sure that EKS Audit Log Monitoring is enabled. Note: Console experience for GuardDuty EKS Runtime Monitoring is now managed as part of the new Runtime Monitoring feature.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s4.png)
#### Runtime Monitoring in Amazon GuardDuty

5. Visit the Runtime Monitoring page under Protection plans in the GuardDuty console.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s5.png)

6. Make sure that Runtime Monitoring is enabled along with Automated agent configuration for each Amazon EKS, AWS Fargate (ECS only), and Amazon EC2.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s6.png)


7. On the Runtime Monitoring page, switch to the tab Runtime coverage. What are the "Coverage statistics"? While out of scope for this workshop, learn more about setting up Runtime Monitoring at https://docs.aws.amazon.com/guardduty/latest/ug/runtime-monitoring.html .
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s7a.png)

*Note*: If the EC2 Instance **Coverage status** is unhealthy and the **Issue** status is "**No Agent Reporting**" or something related to SSM
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/errorSSM.png)
it may be due to SSM agent can not be installed in the EC2 instance. You can clarify by check /var/log/amzn-guardduty-agent in the instance (AL2, AL2023). Further read https://docs.aws.amazon.com/guardduty/latest/ug/gdu-assess-coverage-ec2.html#ec2-runtime-monitoring-coverage-issues-troubleshoot

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s7b.png)

#### Malware Protection for EC2 in Amazon GuardDuty

8. Visit the **Malware Protection** page under Protection plans in the GuardDuty console.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s8.png)

9. GuardDuty automatically initiates a malware scan after generating a finding indicative of malware in an EC2 instance or a container workload. Make sure that GuardDuty-initiated malware scan is enabled.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s9.png)
Here are some EC2 malware scans
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s9b.png)
If you follow a scan through Scan ID
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s9c.png)
EICAR-Test file is a file to check whether the threat detection function works. It is not a real virus.

10. Scroll down and toggle **Retain scanned snapshots when malware is detected** on.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s10.png)
#### RDS Protection in Amazon GuardDuty

11. Visit the **RDS Protection** page under Protection plans in the GuardDuty console.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s11.png)

12. Make sure that RDS Login Activity Monitoring is enabled.


![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s12.png)
#### Lambda Protection in Amazon GuardDuty
13. Visit the **Lambda Protection** page under Protection plans in the GuardDuty console.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s13.png)
14. Make sure that Lambda Network Activity Monitoring is enabled.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.3-GuardDuty-Protection-plans/s14.png)