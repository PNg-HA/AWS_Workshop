---
title : "Security Hub - Findings"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.1.3 </b> "
---

#### Findings in Security Hub

1. Click on **Findings** from the navigation on the left in Security Hub.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s1.png)
2. You can use filters to narrow the list of findings displayed. Click the input **Add filter**.

3. Select **Product name** and then input "GuardDuty" (case-sensitive). Click **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s3.png)
4. This will display all the findings that Security Hub has received from the threat detection service, GuardDuty. There are many Security Hub findings listed here. 
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s4.png)
Try adding a filter to narrow the list down to high severity findings. Click the **Add filters** again.

5. From the dropdown, select **Severity label** and choose is and then input **HIGH**. This is case-sensitive. Click **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s5a.png)
Result:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s5b.png)
6. Pick one of the findings and click on the title. This opens the finding details pane. Expand all the sections and take a few minutes to review the information here. You can see the description, a link to remediation instructions, information about the resource, and more.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s6.png)
7. In the finding details pane click the finding ID link at the top of the pane to display the complete JSON for the finding. The finding JSON can be downloaded to a file if ever needed for further investigation.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s7.png)
8. Close out of the JSON pop-up by clicking the **X** in the top right.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s8.png)
9.  At the top of the finding details, open the **History** tab to view a chronological list of all changes that have been made to the finding. The transparency of finding history helps you identify potential security risks more quickly and take proactive steps to mitigate them.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s9.png)
10. Close the finding details pane by clicking the X in the top right, but stay on the **Findings** page.

11. Try to understand what resources in our environment are generating the most findings. Remove all the filters. Then add a new filter, select **Resource type** and choose is **not** and then input **AwsAccount**. Click **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s11.png)
12. Add another filter. Select **Record state**, choose is, and then input **ACTIVE**. Click **Apply**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s12.png)
13. Click **Add filters** in the search bar again. This time, select **Group by** and choose **ResourceId**. Click **Apply**. This list shows you the number of findings per resource.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s13.png)
14. View detail by clicking **Insight details**:
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s14.png)
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/s15.png)

15. Watch other findings filtered by **TTPs/Discovery/Recon:IAMUser-MaliciousIPCallerCustom** provider type, produced by **GuardDuty**
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o1.png)
16. Click one finding to view more detailed.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o2.png)
It says that an API ListBuckets was invoked by Unauthorized actors, recognized from the IP on the custom threat list bucket.
17. Watch more findings produced by **Inspector**
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.3-Security-Hub-Findings/o3.png)
Those findings are created when **Inspector** detects vulnerabilities by scanning  EC2 instances.