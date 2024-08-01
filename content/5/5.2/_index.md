---
title : "Set up a weekly vulnerability summary email"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: You have been tasked with setting up a weekly summary email to highlight key insights based on the data you are aggregating in Security Hub. As a starting point, the email needs to highlight your current security store, the vulnerabilities impacting the most instances and images, and the highest priority findings.
{{%/notice%}}

{{%notice warning%}}
**Note to Participants**: You must complete the module named "Using insights for prioritization and metrics" before starting this module.
{{%/notice%}}

In this scenario, you'll write a Lambda function to call the Security Hub and Amazon Inspector APIs to fetch the details we need. you'll use EventBridge to call the Lambda weekly, and you will subscribe to the email by using AWS Simple Notification Service.

Amazon Inspector is an automated vulnerability management service that continually scans AWS workloads for software vulnerabilities and unintended network exposure. Vulnerability findings from Inspector are included in the summary email to provide a more comprehensive view of vulnerabilities that may exist in the account that the summary email is being produced for.


#### Configure Amazon Simple Notification Service topic
1. Navigate to [Amazon SNS](https://console.aws.amazon.com/sns/v3/home)


2. Click **Topics** in the left navigation.


3. Click **Create topic**.


4. Select **Standard** type.


5. For the name, enter weekly-vulnerability-email. Leave everything else as is and click **Create topic** at the bottom of the page. This will create the topic.


6. Make a note of the SNS Topic **ARN**. You will need this later. The ARN looks like "arn:aws:sns:us-******:############:weekly-vulnerability-email".


#### Subscribe to the topic

7. From the **weekly-vulnerability-email** topic page, click **Create subscription**.


8. On the **Create subscription** page, under **Protocol**, select **email**.


9. On the **Create subscription** page, under **Endpoint**, enter your email address that you want to use for this workshop to receive notifications. You can unsubscribe at the end of the workshop.


10. Click **Create subscription**.


11. Within a couple minutes, you will receive an email at the email address you entered. Confirm the subscription by clicking "Confirm subscription" in the email.

#### Create the Lambda function to author the email

12. Go to https://console.aws.amazon.com/lambda/home?#/create/function?intent=authorFromScratch 
On the **Create function** page, set the **Function name** to **weekly-vulnerability-email** and pick **Node.js 20.x** for the **Runtime**.


13. Click **Create function**.
Copy and paste the following code into the editor labeled **Code source** (removing the existing code).

```
'use strict';
import {
    SecurityHubClient,
    GetInsightsCommand,
    GetInsightResultsCommand
} from "@aws-sdk/client-securityhub";

import {
    Inspector2Client,
    ListFindingAggregationsCommand
} from "@aws-sdk/client-inspector2";

import {
    SNSClient,
    PublishCommand
} from "@aws-sdk/client-sns";

const securityhubClient = new SecurityHubClient();
const inspectorClient = new Inspector2Client();
const snsClient = new SNSClient();

export const handler = async (event, context) => {
    try {
        let msg = '';
        const getInsights = new GetInsightsCommand({});
        const allCustomInsights = await securityhubClient.send(getInsights);
        const insightsToFetch = [
            ...allCustomInsights.Insights, // include all custom insights
            ...[
                {
                    Name: "Top products by counts of findings",
                    InsightArn: "arn:aws:securityhub:::insight/securityhub/default/28"
                }, // include default insight 28
                {
                    Name: "Severity by counts of findings",
                    InsightArn: "arn:aws:securityhub:::insight/securityhub/default/29"
                } // include default insight 29
            ]
        ];

        const insightsToFetchResults = await Promise.all(insightsToFetch.map(async (o, i) => {
            let getInsightResults = new GetInsightResultsCommand({InsightArn: o.InsightArn});
            let insightResults = await securityhubClient.send(getInsightResults);
            return {...insightResults, Name: o.Name};
        }));
        
        const results = insightsToFetchResults.forEach((o,i) => {
            msg += `------------------------------------\nInsight: ${o.Name}\n------------------------------------\n`;
            o.InsightResults.ResultValues.slice(0,9).forEach((o,i) => {
                msg += `${o.GroupByAttributeValue}: ${o.Count}\n`;
            }); 
            msg += `\nFollow the link below to see all ${o.InsightResults.ResultValues.length} results from this insight in Security Hub:\n`;
            msg += `https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/${o.InsightResults.InsightArn}\n\n`;
        });

        const listFindingAggregationsInput = {
            "aggregationType": "PACKAGE",
            "maxResults": 5,
            "aggregationRequest": {
                "packageAggregation": {
                    "sortOrder": "DESC",
                    "sortBy": "CRITICAL"
                }
            }
        };
        const listFindingAggregations = new ListFindingAggregationsCommand(listFindingAggregationsInput);
        const inspectorFindingAggregations = await inspectorClient.send(listFindingAggregations);
        msg += '\n------------------------------------\nSoftware vulnerabilities impacting the most instances and images:\n------------------------------------\n';
        inspectorFindingAggregations.responses.forEach(pkg => {
            msg += `Package: ${pkg.packageAggregation.packageName}:\nCounts of findings by severity:\n`;
            msg += `Critical: ${pkg.packageAggregation.severityCounts.critical}, `;
            msg += `High: ${pkg.packageAggregation.severityCounts.high}, `;
            msg += `Medium: ${pkg.packageAggregation.severityCounts.medium} \n\n`;
        });

        const emailTopicArn = process.env['emailTopicArn'];
        const messageInput = {
            TopicArn: emailTopicArn,
            Message: msg,
            Subject: "Weekly Vulnerability Report",
        };
        const publish = new PublishCommand(messageInput);
        const publishResponse = await snsClient.send(publish);

        return `Successfully sent message id ${publishResponse.MessageId}`;
    } catch (error) {
        console.error('Error:', error);
        throw error;
    }
}
```
16. Then from the **Code source** menu, click **File** then **Save**. Then click the **Deploy** button.

17. Notice on line 76 of the Lambda code we reference an environment variable where we store the ARN of the SNS Topic. We need to configure that. Directly above the **Code source** section, click the **Configuration** tab. Then click **Environment Variables** from the left navigation appears.

18. Click **Edit** under **Environment Variables**. Then click **Add environment variable**.


19. Input "**emailTopicArn**" under **Key**. Input the ARN of the SNS Topic that you created in step 6 as the **Value**. Make sure there are no spaces.


20.Click **Save**.

#### Set the permissions of the Lambda function
21. Now we need to configure the permissions of our Lambda function so it can run the commands required to send our email. Under the same **Configuration** tab, click **Permissions** from the left navigation. We need to modify the execution role for this Lambda function.


22. Click the **Role name**. It should start with "weekly-vulnerability-email-role-". This will take you to the role in the Identity and Access Management (IAM) console.


23. From the Role page, click the **Add permissions** button and select **Create inline policy**.


24. On the **Create policy** page, click the **JSON** tab. Then replace the existing policy with the following policy. This policy gives the lambda function permissions to retrieve information about Security Hub insights, retrieve vulnerability findings from Inspector, and the ability to send messages to SNS. For this workshop, we will simply allow our Lambda function to publish to any SNS topic, but in any other environment, it is best practice to limit this to specific ARNs.

```
{
	"Version": "2012-10-17",
	"Statement": [
		{
			"Effect": "Allow",
			"Action": [
				"securityhub:GetInsightResults",
				"securityhub:GetInsightFindingTrend",
				"inspector2:ListFindingAggregations",
				"sns:Publish",
				"securityhub:GetInsights"
			],
			"Resource": "*"
		}
	]
}
```

25. Click the **Next** button.


26. Give your policy the name *AllowSNSPublishAndInspectorList* and click **Create policy**.
#### Set timeout of the Lambda function
27. Now we need to configure the timeout of our Lambda function so it has time to run all the commands required to send our email. Under the same **Configuration** tab, click **General Configuration** from the left navigation.


28. Click **Edit**.


29. Set the **Timeout** to **1 min 0 sec**.


30. Click **Save**.


#### Test the vulnerability summary email

31. We are ready to test our vulnerability summary email! Return to the [Lambda console](https://us-east-1.console.aws.amazon.com/lambda/home?region=us-east-1#/functions/weekly-vulnerability-email?tab=code) and open the function you created. 


32. Next to the **Code source** section, click the **Test** dropdown and select **Configure test event**. This should pop open a Configure test event window.



33. Under **Event name**, input "**Generate-Email**". Then click **Save**. We don't need to worry about the Event JSON since we don't use that in our function. You should see a banner appear stating "The test event Generate-Email was successfully saved."


34. Click **Test** again to run the test using your configuration. You'll see the Execution results under the **Code Source** section. Within a couple minutes you should receive an email with your vulnerability summary!


{{%notice warning%}}
**Note to Participants**: If you do not receive an email, check the execution results for an error. Make sure the ARN of the SNS Topic that you configured an environment variable for ends in "weekly-vulnerability-email". If there are any characters after "email", remove them and save the environment variable. Then test again.
{{%/notice%}}


#### Schedule the weekly vulnerability summary email with EventBridge
35. Finally, we need to create an EventBridge rule that will call the Lambda function we made on a weekly basis. Open EventBridge. https://us-east-1.console.aws.amazon.com/events/home 


36. Select **EventBridge Schedule** on the right. Click **Create schedule**.


37. On the **Specify schedule detail** page, name your rule *weekly-vulnerability-email*.


38. Under **Schedule pattern** select "**Recurring schedule**"


39. Leave "**Cron-based schedule**" selected.


40. We'll schedule the email every Monday at 7am CDT. Under the **Cron expression**, set **cron( 0 12 ? * 2 * )**. Select Local time zone from the drop down. You should see a preview of the **Next 10 trigger date(s)**.


41. Under "**Flexible time window**", select "**Off**". Then click **Next**.


42. On the **Select target** step, pick **AWS Lambda - Invoke**. Then select "weekly-vulnerability-email" from the Lambda function dropdown below. Click **Next**.


43. On the **Settings** step, click **Next**.


44. On the **Review and create schedule step**, click **Create schedule**.