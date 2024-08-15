---
title : "Suppressing findings at scale with automations"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 4.2 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: After centralizing all your security alerts and starting prioritization of findings and controls to focus on first, your team has decided that there are too many findings. In an effort to reduce noise, you have been tasked with temporarilly setting up suppression rules for findings that are too low of a priority to be focused on at this time.
{{%/notice%}}

#### Automate finding suppression to reduce noise
Finally, we'll create a rule for suppressing findings. When you are using Security Hub, especially if you have a large environment or use many of the integrations available in Security Hub, you may find it necessary to reduce the volume of findings through suppression. This is only recommended if it helps you focus on the findings that are most important. If you find it necessary to use a suppression rule, you should only suppress low priority findings, and you should make this a temporary measure and revise the suppression rule after you have made progress improving your security posture and reducing the number of findings.


48.  Return to the **Automations** page in Security Hub.


49. Click **Create rule**.


50. Select **Create custom rule**.


51. Change the **Rule name** to "Suppress low severity Inspector findings".



52. Copy the rule name into the **Rule description** or type your own description.


53. For the first **Criteria**, select **ProductName** for **Key**. Select **EQUALS** for **Operator**. Input "Inspector" for **Value**.


54. Click **Add criteria**.



55. For the second Cr**i**teria, select **SeverityLabel** for **Key**. Select **EQUALS** for **Operator**. Select **INFORMATIONAL** for **Value**.



56. Click **Add another value** under the criteria you just added.
![VPC](/images/4/4.2/s56.png)


57. Again, select **EQUALS** for **Operator**. Select **LOW** for **Value**. Now the rule will apply to findings from Inspector with the severity label, INFORMATIONAL or LOW.



58. Click **Refresh** to preview the matching findings.
![VPC](/images/4/4.2/s58.png)

59. Below **Automated action**, select **SUPPRESSED** for **Workflow status**.



60. Scroll down to the **Note**, and input "Automatically suppressed via automation."
![VPC](/images/4/4.2/s60.png)

61. Leave the other default settings and click **Create rule**. If you plan to suppress findings, you should revisit suppression rules on a schedule to ensure they are still necessary and acceptable to your organization. If you need to you can edit or delete automation rules.


62. Try deleting a rule. From the Automations page, check the box next to the rule named "Suppress low severity Inspector findings".

63. Click the **Action** dropdown, and select **Delete**. 

![VPC](/images/4/4.2/s62a.png)
When the confirmation box appears, click **Delete** again.
![VPC](/images/4/4.2/s62b.png)