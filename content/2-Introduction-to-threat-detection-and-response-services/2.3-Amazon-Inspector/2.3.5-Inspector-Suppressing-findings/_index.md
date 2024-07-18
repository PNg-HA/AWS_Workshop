---
title : "Inspector - Suppressing findings"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.3.5 </b> "
---

#### Configure a rule to suppress low vulnerability findings in dev



1. Click on **Suppression rules** from the navigation on the left in Inspector.


2. Click on **Create Rule**.


3. Enter "**Low Severity - Dev Environment**" in the **Name** field.



4. Enter "**Suppress all findings with a low severity score for resources tagged with Development**" in the **Description** field.


5. Under **Suppression rule filters** select **Severity** and check the box for **Low**. Click **Apply**.



6. Then click to add another filter and select **Resource tag** under EC2. Input "**Environment**" for the key and "**Development**" for the **value**. Click **Apply**.


7. Click **Save**.


8. You can view suppressed findings in the **All Findings** page by switching from **Active** to **Suppressed** in the dropdown.