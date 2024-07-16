---
title : "GuardDuty - Findings"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 2.2.2 </b> "
---

A GuardDuty finding represents a potential security issue detected within your network. GuardDuty generates a finding whenever it detects unexpected and potentially malicious activity in your AWS environment. You can view and manage your GuardDuty findings on the Findings page in the GuardDuty console or by using the GuardDuty CLI or API operations.

#### Review a GuarDuty finding
1. Navigate to **Findings** in Amazon GuardDuty by clicking **Findings** in the left hand navigation.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s1.png)

2. Select one of the findings on the page by clicking on the row. This will open the finding summary on the right side of the page. Finding details vary based on finding type.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s2.png)

3. Review the finding that you opened. 


#### Understanding GuardDuty finding severity

4. Find the **Severity** of the finding you selected.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s4.png)


5. If you want to view or download the finding in JSON form, you can click the **Finding ID** at the top of the finding summary.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s5a.png)

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s5b.png)


6. Click the **X** in the top right of the finding summary to close it.

#### Searching and Filtering GuardDuty Findings

7. Click on the Search bar where it says **Add filter criteria**.



8. Type **Severity** in the search bar and click on **Severity**, which then opens to a sub-menu with Low, Medium, and High options.


9. Check the box for **High** and click **Apply**. This will then update the list of displayed findings accordingly.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s9a.png)

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s9b.png)
#### Managing GuardDuty findings
10. With a finding selected, click the **Actions** dropdown (top right of the page). Click **"Archive"** to archive the finding.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s10.png)

11. Archiving the finding will hide it from the list of Current findings. To view it, click the **"Current"** dropdown, and select **"Archived"** to see the finding you just archived.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s11a.png)

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s11b.png)

12. To unarchive the finding, select it, and then click the **Actions** dropdown again (top right of the page). This time, click **"Unarchive"** to unarchive the finding.


![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.2-GuardDuty-Findings/s2.png)