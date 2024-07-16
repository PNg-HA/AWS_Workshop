---
title : "GuardDuty - Suppressing findings"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.2.5 </b> "
---

#### Configure a rule to suppress unwanted findings for your vulnerability assessment tool

1. From the GuardDuty console, open the **Findings** page.


2. Click the button, **Suppress Findings**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s2.png)



3. The suppression rule should consist of two filter criteria. The first criteria should use the Finding type attribute with a value of Recon:EC2/Portscan. In the field **Add filter criteria**, select **Finding Type** and type **"Recon:EC2/Portscan"**. Click **Apply**.

![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s3.png)

4. Notice that after adding the first filter, the preview at the bottom of the page shows the finding(s) that would be suppressed by this rule. To ensure that you don't accidently suppress a finding you want to be alerted to, add a second filter criteria to narrow the rule.


5. The second filter criteria should match the instance or instances that host these vulnerability assessment tools. You can use either the Instance image ID attribute or the Tag value attribute depending on which criteria are identifiable with the instances that host these tools. Click **Add filter criteria** again, and select **Instance Tag Value** and type "**Scanner**". Click **Apply**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s5.png)


6. Notice that only one finding will be suppressed by this rule.


7. Enter "**VulnerabilityAssessmentTool**" in the **Name** field.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s7.png)


8. Enter "**Suppress findings from Vulnerability Assessment Tool.**" in the **Description** field.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.2-Amazon-GuardDuty/2.2.5-GuardDuty-Suppressing-findings/s8.png)


9. Click **Save**.