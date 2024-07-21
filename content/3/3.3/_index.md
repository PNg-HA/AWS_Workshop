---
title : "Centralizing findings from AWS partner solutions"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 3.3. </b> "
---
AWS Security Hub can aggregate security finding data from several AWS services and from supported AWS Partner Network (APN) security solutions. This aggregation provides a comprehensive view of security and compliance across your AWS environment.

You can also send findings that are generated from your own custom security products.

From the supported AWS and partner product integrations, Security Hub receives and consolidates only findings that are generated after you enable Security Hub in your AWS accounts. The service does not retroactively receive and consolidate security findings that were generated before you enabled Security Hub.

One of the partner integrations available is Cloud Custodian. Cloud Custodian enables you to manage your cloud resources by filtering, tagging, and then applying actions to them. The YAML DSL allows definition of rules to enable well-managed cloud infrastructure that's both secure and cost optimized. For more information, visit https://cloudcustodian.io/ 

#### Receive findings from Cloud Custodian
1. Open the **Integrations** page in Security Hub and search for **Cloud Custodian**.
![VPC](/images/3/3.3/s1.png)

2. Click **Accept Findings**. Review the permissions required for the integration.

![VPC](/images/3/3.3/s2.png)
3. Click **Accept findings**. This will put in place a service policy allowing the partner solution to send finding information into this account. For the purposes of this workshop a Cloud Custodian instance is already set up to automatically send findings to the integration you just enabled. To use other partner integrations in your account, you would still need to complete the Configure step in the partner solution so findings that are created by the partner solution are sent to Security Hub.
Now accepting findings

You can click on See findings to view new findings from Cloud Custodian. It can take 5-10 minutes for them to appear.