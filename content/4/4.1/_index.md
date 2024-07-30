---
title : "Prioritizing findings at scale with automations"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 4.1 </b> "
---

#### Automate elevating finding severity
The first automation you will set up will be to elevate the severity of priority findings for business critical applications. Specifically, we want to elevate the severity of all GuardDuty findings for EC2 instances tagged for production.

1. Open the **Automations** page in Security Hub.


2. Click **Create rule**.


3. For this automation rule, we'll start with a template. Leave "Create a rule from template" selected and under **Rule template** pick **Elevate severity of findings that relate to important resources**.


4. Change the **Rule name** to "Elevate severity of GuardDuty findings that relate to production EC2 instances".


5. Copy the rule name into the **Rule description** or type your own description.


6. Take a moment to review the rule. We'll need to change some of the criteria for our use case.



7. Change the **Value** of the first criteria (Key: ProductName) to "GuardDuty".


8. Click **Remove** next to the criteria, Key: ComplianceStatus.


9. Click **Remove** next to the last criteria (Key: ResourceId).



10. Click **Add criteria**. For **Key** select **ResourceTags**. For **Operator** select **EQUALS**. For **Value**, input "Environment" into the top box and "Production" into the bottom box (this is the key-value pair for the tag).


11. In the **Preview of findings that match criteria** click **Refresh** to make sure you configured everything correctly. You should see a preview of the existing findings that meet the criteria.


12. For this rule, we'll elevate all the matching findings to HIGH severity. If implementing this for your organization, you may want an additional rule to elevate findings from GuardDuty that are originally HIGH to CRITICAL. In the section, **Automated action**, set Severity to **HIGH**.



13. Leave the other default settings and click **Create rule**.


#### Automate adding user defined fields to production alerts
There are circumstances where you may want to identify alerts on production resources, but not elevate the severity. One approach to this is adding notes and user defined fields. The next automation you will set up will be to add notes and user defined fields to any findings from an accounts we identify as production.

14. Return to the **Automations** page in Security Hub.


15. Click **Create rule**.


16. For this automation rule, we'll select **Create custom rule**.


17. Change the **Rule name** to "Tag production findings".


18. Change the **Rule description** to "Tag findings for resources in production accounts".


19. For this rule, we only need one criteria. Set the **Key** to **AwsAccountId**. Set **Operator** to **EQUALS**. Set **Value** to the ID of the account you are in right now. The account ID can be found in the top right-hand corner of the AWS console. Make sure there are no spaces or dashes when copying the account ID.


20.  Click **Refresh** to preview the findings that meet the criteria.

**Note to Participants**: Do to limitations in Workshop Studio, you only have one account, so this automation rule will apply to every one of your findings. However, in a multi-account architecture where you have set up a delegated admin through AWS Organization, you will be able to follow the same instructions and identify one or more accounts, selectively, as production.

21. Scroll down to **Automated** action. For **Note** input "This finding is in a production account (identified via automation rule)".



22. Click **Add another key-value pair**. For **Key** input "Environment". For **Value** input "Production".



23. Leave the other default settings and click **Create rule**.



24. If you want to review a rule you created, you can click the **Name** of the rule from the Automations page. Click the name **Tag Production findings**.


#### Automate adding user defined fields to findings aligned to organization goals
Next, set up automation similar to the last automation rule, but instead of adding user-defined fields to findings in production accounts, add them to findings that meet the criteria of an organization goal. In this case, the organization goal will be to secure AWS accounts and IAM. To implement this, your automation criteria will be findings from Security Hub controls for "Account" and "IAM". 

35. (The index 35 is from Workshop, not a mistake from the author) Return to the **Automations** page in Security Hub.


36. Click **Create rule**.


37. Select **Create custom rule**.



38. Change the **Rule name** to "Tag findings for security goal".



39. Change the **Rule description** to "Tag findings for security goal, secure IAM and accounts.".


40. We want our criteria to match all findings for Security Hub controls for "Account" or "IAM". Under Criteria, select GeneratorId for Key.


41. Select **PREFIX** for **Operator**. By selecting "PREFIX" we don't need criteria for every individual control. If you have enabled "Consolidated control findings" (which is the case for this account), you will just need to look for 2 control ID prefixes.


42. For **Value**, input "security-control/Account". This will match any finding for a Security Hub control ID starting with "security-control/Account".



43. Below the **Value** input box, click **Add another value**. This will create a second Operator and Value input for the same Key.


44. For the second **Operator** and **Value**, input **PREFIX** and "security-control/IAM", respectively.


45. Click Refresh to preview the matching findings.



46. Under Automated action, click **Add another key-value pair**. For Key, input "security-goal". For **Value**, input "account-and-iam".


47. Leave the other default settings and click **Create rule**. Remember, you can use user-defined fields for filtering findings and creating insights in Security Hub if you want an easy we way track goals at an organization and account level.

#### Review findings updated by automation rules

48. Navigate to the **Findings** page in Security Hub.


49. Look for a finding that has the comment icon next to the Updated at time.


50. Click the title of a finding that has the comment icon next to the Updated at time to open the finding details.


51. Expand the **Notes** section to see the note created by our automation rule.


52. Click the **History** tab at the top of the finding details. You can see the timestamp of when and how the finding was updated by the automation rule.