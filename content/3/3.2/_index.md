---
title : "Aggregating findings from multiple AWS accounts"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 3.2. </b> "
---
When you use both Security Hub and AWS Organizations together, you can automatically enable Security Hub for all of your accounts, including new accounts as they are added. This increases the coverage for Security Hub checks and findings, which provides a more comprehensive and accurate picture of your overall security posture. By using the delegated administrator feature of Security Hub along with central configuration, you achieve a centralized view of your security on AWS across AWS services and partner services, across accounts, and across regions.

**Note to readers**: Although the workshop says "At this time, we are unable to demonstrate multi-account aggregation in this workshop". However, in the **Automated Security Response on AWS** section, the architecture diagram shows that the Cloudformation stacks have been deployed in multiple AWS accounts.