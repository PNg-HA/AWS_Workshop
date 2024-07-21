---
title : "Inspector - Overview"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.3.1 </b> "
---


#### Inspector
Amazon Inspector is a vulnerability management service that continually scans AWS workloads for software vulnerabilities and unintended network exposure. With a few steps in the AWS Management Console, you can use Amazon Inspector across all accounts in your organization. Once started, it automatically discovers Amazon Elastic Compute Cloud (EC2) instances, container images residing in Amazon Elastic Container Registry (ECR) and within continuous integration and continuous delivery (CI/CD) tools, and AWS Lambda functions, at scale, and immediately starts assessing them for known vulnerabilities.

Amazon Inspector calculates a highly contextualized risk score for each finding by correlating common vulnerabilities and exposures (CVE) information with factors such as network access and exploitability. This score is used to prioritize the most critical vulnerabilities to improve remediation response efficiency. All findings are aggregated in the Amazon Inspector console and pushed to AWS Security Hub and Amazon EventBridge to automate workflows. Vulnerabilities found in container images are also sent to Amazon ECR for resource owners to view and remediate. Amazon Inspector empowers security teams and developers of any size to achieve comprehensive infrastructure workload security and compliance across their AWS environments.

![VPC](/images/2/2.3/2.3.1/s1.png)