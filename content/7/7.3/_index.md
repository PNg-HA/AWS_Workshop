---
title : "Using insights for prioritization and metrics"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 4.3 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: Since your team turned on various AWS security services and aggregated everything in Security Hub, your leadership is asking for insights from all that data. In particular, you have been asked to glean insights on compliance, highest priority alerts, and resources generating the highest number of security alerts.
{{%/notice%}}

An AWS Security Hub insight is a collection of related findings. It identifies a security area that requires attention and intervention. For example, an insight might point out EC2 instances that are the subject of findings that detect poor security practices. An insight brings together findings from across finding providers.

Each insight is defined by a group by statement and optional filters. The group by statement indicates how to group the matching findings, and identifies the type of item that the insight applies to. For example, if an insight is grouped by resource identifier, then the insight produces a list of resource identifiers. The optional filters identify the matching findings for the insight. For example, you might want to only see findings from specific providers or findings that are associated with specific types of resources.

Security Hub offers several built-in managed insights. You cannot modify or delete managed insights.

An insight only returns results if you have enabled integrations or standards that produce matching findings. For example, the managed insight 29. Top resources by counts of failed CIS checks only returns results if you enable the CIS AWS Foundations standard.

#### Review the built in managed insights
1. In Security Hub, navigate to the **Insights** page using the navigation on the left. Take a couple minutes to review the built-in insights.


2. From the Insights page, find the insight named **Insight: 23. Top products by counts of findings**, and click the title to open it. https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/arn:aws:securityhub:::insight/securityhub/default/28 

![VPC](/images/4/4.3/s2.png)

3. This insight highlights how many active findings are aggregated from each product you have already integrated. Understanding your use case for each of the integrated products, this is helpful for measuring your organizations efforts to drive down the number of active vulnerabilities, threats, and resource-level compliance violations.
![VPC](/images/4/4.3/s3.png)


4. From the Insights page, find the insight named **Insight: 24. Severity by counts of findings**, and click the title to open it. https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/arn:aws:securityhub:::insight/securityhub/default/29 



5. This insight highlights the number of active findings in your environment by severity label. These are useful numbers for reporting incremental improvement in your environment. Try to drive these numbers down over time starting with the number of findings that have a CRITICAL or HIGH severity label.
![VPC](/images/4/4.3/s5.png)

#### Create custom insight to track number of active threats

6. To track security issues that are unique to your AWS environment and usage, you can create custom insights. You will create custom insights to track your organization's key metrics. The first insight you've decided to create will highlight the number of active threats in your environment grouped by threat purpose, resource type affected, overall malicious activity, etc. Return to the **Insights** page and click Create insight.


7. Click **Add filter**. Under Filters, select **Product name**, and then input "GuardDuty" (it is case-sensitive). We are filtering findings down to those from GuardDuty because that is the threat detection service you are using to continuously monitor your AWS accounts and workloads for malicious activity.


8. Click **Add filter** again, but this time select **Group** by under Grouping. Then select **Type**. One of the most useful pieces of information provided for findings by GuardDuty is the finding type. The purpose of the finding type is to provide a concise yet readable description of the potential security issue. By selecting "Group by Type" your insight will give you the count of findings matching each finding type.


9. Click **Create insight**. For insight name, input "Security alerts by GuardDuty finding type".
![VPC](/images/4/4.3/s9.png)

![VPC](/images/4/4.3/s9b.png)
10. You can click **Insight details** to view the results charted at visualizations. Insights can also be fetched via Security Hub's API.
![VPC](/images/4/4.3/s10.png)

11. Return to the **Insights** page that lists out all of your insights.



12. Realizing this is rather easy to set up, you should consider how to accomplish the same in every account. The answer is to use infrastructure-as-code. Since that is how to want to set up insights long-term, go ahead and delete the custom insight you just created. Search the list of insights for "Security alerts by GuardDuty finding type".


13. When you find it, select the small menu icon on the tile, and then click **Delete**.


#### CloudFormation to deploy custom insights in Security Hub
{{%notice info%}}
**Scenario / Problem Statement**: Having demonstrated how easy it is to get insights from Security Hub, you have been challenged to author infrastructure-as-code to deploy Security Hub custom insights to all of your organizations accounts, so each account owner can see the same insights for their individual account that the security team is rolling up for the entire organization.
{{%/notice%}}

AWS CloudFormation lets you model, provision, and manage AWS and third-party resources by treating infrastructure as code. For the purposes of this module, you can use the following CloudFormation code to implement several more custom insights. You are welcome to modify it and experiment. This example template assumes Security Hub is already enabled. To make this module slightly easier, the CloudFormation template has already been staged in an S3 bucket for you.


```
AWSTemplateFormatVersion: 2010-09-09
Description: Example template to create a Security Hub insights
Resources:
  SecurityHubInsightGDFindings:
    Type: 'AWS::SecurityHub::Insight'
    Properties:
      Name: Security alerts by GuardDuty finding type
      Filters:
        ProductName:
          - Value: GuardDuty
            Comparison: EQUALS
        WorkflowStatus:
          - Value: NEW
            Comparison: EQUALS
          - Value: NOTIFIED
            Comparison: EQUALS
        RecordState:
          - Value: ACTIVE
            Comparison: EQUALS
      GroupByAttribute: Type
  SecurityHubInsightSHControls:
    Type: 'AWS::SecurityHub::Insight'
    Properties:
      Name: Most Failed Security Hub Controls
      Filters:
        ProductName:
          - Value: Security Hub
            Comparison: EQUALS
        WorkflowStatus:
          - Value: NEW
            Comparison: EQUALS
          - Value: NOTIFIED
            Comparison: EQUALS
        RecordState:
          - Value: ACTIVE
            Comparison: EQUALS
      GroupByAttribute: GeneratorId
  SecurityHubInsightMostSev:
    Type: 'AWS::SecurityHub::Insight'
    Properties:
      Name: Resources with the most security alerts Sev40+
      Filters:
        SeverityNormalized:
          - Gte: 40
            Lte: 100
        ResourceType:
          - Value: AwsAccount
            Comparison: NOT_EQUALS
        WorkflowStatus:
          - Value: NEW
            Comparison: EQUALS
          - Value: NOTIFIED
            Comparison: EQUALS
        RecordState:
          - Value: ACTIVE
            Comparison: EQUALS
      GroupByAttribute: ResourceId
```
The template above creates three custom Security Hub insights:
+ Security alerts by GuardDuty finding type
+ Most failed Security Hub controls
+ Resources with the most security alerts (medium-critical severity)

#### Deploy your CloudFormation template with custom insights

14. Since the CloudFormation above has already been staged in S3, open the [Stacks](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/?filteringText=cfn&filteringStatus=active&viewNested=false)  page in the CloudFormation console.


15. Click the name of the stack, **cfn**.


16. Open the **Outputs** tab.
![VPC](/images/4/4.3/s16.png)


17. Open "InsightsCloudFormationLink" in a new tab to launch the preconfigured CloudFormation template in us-east-1 (N. Virginia). This will open the "Create stack page" in us-east-1 (N. Virginia) and populate the link to the CloudFormation template in the form. Make sure it says "N. Virginia" in top right of the page.



18. Click **Create stack** at the bottom of the page.
![VPC](/images/4/4.3/s18.png)


19. Wait for the template to complete deployment.



20. Return to Security Hub and open the **Insights** page. 
![VPC](/images/4/4.3/s18b.png)
Find and review the insights you just created by switching the dropdown menu at the top of the page to **Custom insights**. For example:

![VPC](/images/4/4.3/s18c.png)