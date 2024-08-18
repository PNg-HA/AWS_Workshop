---
title : "Inspector  - Findings"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 2.3.3 </b> "
---

#### Filter findings

1. Navigate to **Findings: All Findings** (https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/findings/all ).


2. Click the input at the top of the page labeled **Filter criteria** that has the placeholder "Add filter". A finding filter allows you to view only the findings that match the criteria you specify. Findings that do not match the filter criteria are excluded from your view. Take a minute to review all the options for filtering your list of findings.



3. You can create a filter on each tab to focus on specific types of findings. Try filtering on a resource tag. Click the field, **Add filter**. Select **Resource tag**. Keep in mind, filters are case sensitive. Input **"Environment"** for Key. Input "**Development**" for **Value**. Click **Apply**. This shows findings for resources with a certain tag applied.

![VPC](/images/2/2.3/2.3.3/s3.png)

4. Now remove that filter you just added by clicking **Clear filters**.

<!-- 

5. Add a new filter. Click the field, **Add filter**. Select **Resource tag**. Keep in mind, filters are case sensitive. Input "**Name**" for Key. Input "**EC2InstanceDev3**" for **Value**. This will display only findings for EC2 instance(s) named "EC2InstanceDev3". You can optionally try adding multiple filters at the same time to narrow your search.



6. Now remove that filter(s) you just added by clicking **Clear** filters.


#### Reviewing a network reachability finding for EC2

7. Click the field, **Add filter**. Select **Type** and then select **Network Reachability**. Click **Apply**.


8. Click the title of one of the filtered findings to view the finding report. In the finding report, you can see information such as Severity, Open port range, Account ID, information about the resource, and Open Network Paths.


#### Reviewing a code vulnerability finding for Lambda
9. Remove all the filters by clicking **Clear filters**. Click the field, **Add filter**. Select **Type** and then select **Code Vulnerability**. Click **Apply**.


10. Click the title of finding, **CWE-117,93 - Log injection**, to view the finding report.



11. The finding states "User-provided inputs must be sanitized before they are logged. An attacker can use unsensitized input to break a log's integrity, forge log entries, or bypass log monitors." The details in the report include the information about the resource and suggested remediation. Read the suggested remediation. As a challenge, come back later if you have time and try to remediate the issue WITHOUT deleting the function.


12. Close out of the finding and remove all the filters by clicking **Clear filters** again. -->