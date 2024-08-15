---
title : "Centralizing findings from AWS security services"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 3.1. </b> "
---

Security Hub automatically collects and consolidates findings from AWS security services enabled in your environment, such as intrusion detection findings from Amazon GuardDuty, vulnerability scans from Amazon Inspector, Amazon Simple Storage Service (Amazon S3) bucket policy findings from Amazon Macie, publicly accessible and cross-account resources from IAM Access Analyzer, and resources lacking WAF coverage from AWS Firewall Manager.


This aggregation provides a view of security and compliance across your AWS environment. AWS Security Hub aggregates security findings from dozens of AWS security services and APN products in a single place and format, the AWS Security Finding Format (ASFF). Security Hub doesn't retroactively detect and consolidate security findings that were generated before you enabled Security Hub. All findings are stored in Security Hub for 90 days after last update date.

The Integrations page in the AWS Management Console provides access to all of the available AWS and third-party product integrations. The AWS Security Hub API also provides operations to allow you to manage integrations. For AWS services, Security Hub automatically enables the integration, and you can optionally disable each integration.

#### Centralizing findings from AWS security services
1. Navigate to Security Hub and open the Summary page. The [Security Hub Summary page](https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/summary) gives you an overview of security and compliance status of your AWS account.  


2. Take a few minutes to review the insights generated on the Summary page. Can you find the most common threat types identified in your environment? These findings are being aggregated in Security Hub from Amazon GuardDuty.
Security Hub widgets
![VPC](/images/3/3.1/s2.png)
3. Open the Integrations page in Security Hub. Scroll through the integrations available. 
![VPC](/images/3/3.1/s3a.png)
![VPC](/images/3/3.1/s3b.png)
As you can see, many of the integrations, including with "Amazon: GuardDuty" and "Amazon: Inspector", are automatically enabled for you. However, you still need to have each service turned on (in addition to the integration) to generate the respective findings.

4. Security Hub provides you with the flexibility to enable and disable AWS integrations. Although not usually recommended, try disabling the AWS: Firewall Manager integration. Click Stop accepting findings in the box labeled AWS: Firewall Manager.
![VPC](/images/3/3.1/s5a.png)
5. You will be presented with a popup stating "Not accepting findings from this integration means that the product can still try to send findings to Security Hub, but Security Hub will not ingest them." Click Cancel. We should leave this integration on.

6. To see all the security findings from an individual service, you can click use this page to see that. Find the integration for "Amazon: GuardDuty" and click See findings. This will open the findings page and add a filter so you are only looking at findings from Amazon GuardDuty. Stay on the Findings page.

7.  You can optionally remove or change some of the filters to adjust the list of filtered findings you are looking at.

#### Searching and reviewing security findings
8. To see all of the findings in Security Hub, remove the filters (and Group By, if one is listed) at the top of the page by clicking the X next to each of the filters.

9. There are many Security Hub findings listed here. Try adding a filter to narrow the list down to high severity findings from the threat detection service, GuardDuty. Click Add filters in the search bar.

10. From the dropdown, select Severity label and choose is and then input HIGH. This is case-sensitive. Click Apply.
![VPC](/images/3/3.1/s10a.png)
Result:
![VPC](/images/3/3.1/s10b.png)
Choose one finding to see more information
![VPC](/images/3/3.1/s10c.png)
11. Click Add filters in the search bar again. This time, select Product name and choose is and then input GuardDuty. Click Apply.

12. Pick one of the findings and click on the title. This opens the finding details pane. Expand all the sections and take a few minutes to review the information here. You can see the description, a link to remediation instructions, information about the resource, and more.



13. In the finding details pane click the finding ID link at the top of the pane to display the complete JSON for the finding. Security Hub processes findings using a standard findings format called the AWS Security Finding Format (ASFF), which eliminates the need for time-consuming data conversion efforts. The finding JSON can be downloaded to a file if ever needed for further investigation.
![VPC](/images/3/3.1/s13.png)

14.  Close out of the JSON pop-up by clicking the X in the top right.

15.  At the top of the finding details, open the History tab to view a chronological list of all changes that have been made to the finding. The transparency of finding history helps you identify potential security risks more quickly and take proactive steps to mitigate them.

16.  Close the finding details pane by clicking the X in the top right, but stay on the findings page.

17.  Let's try to understand what resources in our environment are generating the most findings. Remove all the filters. Then add a new filter, select Resource type and choose is not and then input AwsAccount. Click Apply.
![VPC](/images/3/3.1/s17a.png)
Result:
![VPC](/images/3/3.1/s17b.png)
18.  Add another filter. Select Record state, choose is, and then input ACTIVE. Click Apply.
![VPC](/images/3/3.1/s18.png)
19.  Click Add filters in the search bar again. This time, select Group by and choose ResourceId. Click Apply. This list shows you the number of security findings per resource. This can help you prioritize work by the resources in your environment with the most security issues.
![VPC](/images/3/3.1/s19.png)
Result:
![VPC](/images/3/3.1/s19b.png)
#### Generating insights from aggregated findings
20. If you find the list of number of security findings per resource beneficial, you can save it as a custom Insight for future reference. Click Create insight in the top right of the screen.

21. Name the insight "0. Resources by counts of findings". Then click Create insight.
![VPC](/images/3/3.1/s20.png)
22. In the navigation on the left, click Insights. A Security Hub Insight is a collection of related findings defined by an aggregation statement and optional filters. An insight identifies a security area that requires attention and intervention. Security Hub offers several managed (default) insights that you can't modify or delete. You can also create custom insights to track security issues unique to your AWS environment and usage.

23. The first Insight listed is the one you just created. Click on the title 0. Resources by counts of findings. Here you have the same view as before plus generated graphs. We'll dive deeper into Insights later.
![VPC](/images/3/3.1/s23.png)
24. Return to the Insights page using the navigation on the left.

25. Take a few minutes to review the other AWS-provided Insights. Filter for insight severity.

26. Click on 24. Severity by counts of findings.

27. Select a Severity Label (e.g. Critical) to see the underlying finding(s).

![VPC](/images/3/3.1/s26.png)
Result:
![VPC](/images/3/3.1/s26b.png)