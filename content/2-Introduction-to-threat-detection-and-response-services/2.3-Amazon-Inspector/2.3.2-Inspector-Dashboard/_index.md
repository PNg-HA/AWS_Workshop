---
title : "Inspector - Dashboard"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.3.2 </b> "
---

#### Check out the Inspector dashboard

1. Navigate to the Inspector **Dashboard**. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/dashboard 


2. At the top of the page you can see the **Environment coverage**. Notice that only a percentage of Instances are covered. You can optionally click on the percentage to dive deeper into which of your instances are being scanned.



3. On the Dashboard, check out **Risk Based Remediations** near the top of the page. 



<!-- The risk-based remediations section shows the top five software packages with critical vulnerabilities that impact the most resources in your environment. Remediating these packages can significantly reduce the number of critical risks to your environment. Choose the software package name to see associated vulnerability details and impacted resources. -->



4. On the Dashboard, you can also review:
- Amazon ECR repositories with most critical findings
- Amazon ECR container images with most critical findings
- Amazon EC2 Instances with most critical findings
- Amazon Machine Images (AMI) with most critical findings
- AWS Lambda functions with the most critical findings
- Code detectors for Lambda code scanning