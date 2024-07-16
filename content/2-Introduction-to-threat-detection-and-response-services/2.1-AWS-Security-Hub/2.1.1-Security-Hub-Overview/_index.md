---
title : "Security Hub - Overview"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 2.1.1 </b> "
---


#### Security Standards
1. Navigate to the Summary page in Security Hub. [AWS Security Hub summary page](https://console.aws.amazon.com/securityhub/home?#/summary)

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s1.png) 

2. From the Summary page, you can see what standards have been enabled in your account, and you can see the number of passed and failed controls per standard. The percentage is the percent of controls in a "passed" compliance status compared to the number of controls that have a "passed" or "failed" compliance status.

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s2.png)

3. Click on **Security standards** from the navigation on the left.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s3.png)

1. Standards were enabled using CloudFormation when the workshop was provisioned.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s4.png)

1. Let's dive into one of the standards. Click **View Results** for NIST Special Publication 800-53 Revision 5.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s5.png)

#### Controls
6. From the top of the page, you can select tabs to show only "Passed", "Failed", "Disabled", etc. controls. You can also filter to a control using the search bar that says "Filter all enabled controls". Click the search bar and filter on ID. Specify **ID = EC2.13**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s6.png)

7. Click on the Title: **Security groups should not allow ingress from 0.0.0.0/0 to port 22**. This presents a view of all resources evaluated for this particular control and the current status of each resource as it relates to the control. Notice there are some resources with a **FAILED** status and some with a **PASSED** status.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s7.png)

8. AWS Security Hub Security Standards provide remediation instructions for each check. At the top of the page click the **Remediation instructions link** to open guidance in a new tab.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s8.png)

#### Investigating a misconfigured resource

9. Return to the Security Hub control you were looking at. For one of the **FAILED** resources open **Config rule** in the **Investigate** column. This will display links that will take you to AWS Config to view the configuration timeline for this resource or the overall config rule that performed the evaluation on this resource.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s9.png)

10. Click **Configuration timeline**. This opens AWS Config, and shows the resource configuration timeline for this security group. From here, you can investigate the point in time when the resource became non-compliant by reviewing the timestamped events. If you want, take a minute to expand a couple of the events.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s10.png)
11. Expand a **CloudTrail Event** listed.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s11.png)
12. Then click **CloudTrail** under View event. This opens up that event, logged in CloudTrail. AWS CloudTrail monitors and records account activity across your AWS infrastructure, giving you control over storage, analysis, and remediation actions.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s12.png)
#### Consolidated controls
13. Return to Security Hub and open General settings page from the navigation panel. https://console.aws.amazon.com/securityhub/home?#/settings/general 
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s13.png)
14.  At the top of the page, you should see that both "Auto-enable new controls in enabled standards" and "Consolidated control findings" is enabled.

15.  Click Controls from the navigation panel. This page shows a similar view to what you reviewed at the standard level, but here you see the full list of consolidated controls. This makes it easier to see how many individual controls in your account are passing, failing, or in some other state. Additionally, with consolidated findings enabled, controls included in multiple enabled standards no longer generate a finding per standard (duplicates). This helps manage the number of findings in your environment and removes potential confusion around duplicate findings.

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s15.png)

#### Insights
16. In the navigation on the left, click **Insights**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.1-security-hub-overview/s16.png)
17. In the search bar, input **"severity"** and select the insight named **24. Severity by counts of findings**.
![VPC](/images/2/2.1-AWS-Security-Hub/s17.png)
18. Security Hub offers several built-in managed insights. You cannot modify or delete managed insights. **24. Severity by counts of findings** is one of the managed insights. Take a minute to review the number of findings in the environment. Select a **Severity Label** (e.g. Critical) to see the underlying finding(s).
![VPC](/images/2/2.1-AWS-Security-Hub/s18.png)