---
title : "Building your own Security Hub integration"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 3.5. </b> "
---
In this module, you will build a custom solution to send findings from Security Hub to a Slack channel. To accomplish this, you will set up a custom action that will send the details of a Security Hub Finding to a Lambda function (via EventBridge) that will format the message and then post into a Slack security alerts channel. Once the setup is complete, findings will flow from left to right in the below diagram.

#### Setup Slack workspace
1. For the purposes of this workshop, we recommend setting up a new temporary Slack workspace. Use the following link to create a new Slack workspace: https://get.slack.help/hc/en-us/articles/206845317-Create-a-Slack-workspace 


2. When you have finished creating your Slack workspace, create a new channel named "security-hub-alerts". This is where we will send notifications from Security Hub using the custom integration. For this exmaple, set the channel visibility to public.

#### Setup Slack application for integration
3. Go to the Slack API site, https://api.slack.com.


4. At the top of the page, click Your apps. Then click Create an App.


5. In the Create an app popup. choose From scratch.



6. For App Name input "security-hub-to-slack". For Pick a workspace to develop your app in select the workspace you just created.


7. Click **Create App**.


8. Open **Incoming Webhooks**.


9.  On the **Activate Incoming Webhooks** page, switch the toggle to ON.


10. Then at the bottom of the page, click **Add New Webhook to Workspace**.


11. On the page that asks, "Where should security-hub-alerts post?" Select **#security-hub-alerts**. Click **Allow**.
Back on the Incoming Webhooks page, copy the **Webhook URL** you just created. You will need this later.


#### Create Custom Action in Security Hub
Slack is setup. Now you need to configure the Security Hub custom action, EventBridge rule, and Lambda function to push findings to your Slack app (via the webhook).

13. Return to Security Hub. https://console.aws.amazon.com/securityhub/ 


14. Open the Custom actions page from the navigation on the left under Management.


15. Click Create custom action.


16. For Action name input "Send to Slack". For Description input "Custom action to send findings to Slack." For Custom action ID input "SendToSlack".


17. Copy the Custom action ARN that was generated for Send to Slack. You will need this later.
#### Create the EventBridge Rule


18. Navigate to **Amazon EventBridge**.



19. Click on the **Create rule** on the right side.


20. In the **Define rule detail** page give your rule a **name** and a **description** that represents the rule's purpose (for example, the same name and description you used for the custom action). Then click **Next**.


21. All Security Hub findings are sent as events to the AWS default event bus. The define pattern section allows you to identify filters to take a specific action when matched events appear. On the **Build event pattern** step, leave the **Event source** set to **AWS events or EventBridge partner events**.

22. Scroll down to Event pattern. Under Event source, leave it set to AWS Services, and under AWS Service, select Security Hub.


23. For the Event Type, choose Security Hub Findings – Custom Action.



24. Then select the Specific custom action ARN(s) radio button and enter the ARN for the custom action that you just created.



25. Click Next.



26. On the Select target(s) step, select Lambda function from the Select a target dropdown. Then select security-hub-to-slack from the Function dropdown.



27. The Lambda function, security-hub-to-slack, was created using CloudFormation before the start of the lab. If you want to review the code, you can see it in the Lambda console. Click Next.


28. On the Configure tags step, click Next.



29. On the Review and create step, click Create rule.



#### Customize the Lambda function to send messages to your Slack channel



30. Navigate to the Lambda console and open the Functions page.



31. Open the security-hub-to-slack function by clicking on the name.




32. On the function page, open the Configuration tab and then choose Environment variables.




33. Click the **Edit** button. Set slackChannel to "security-hub-alerts". Set webHookUrl to the webhook you copied earlier in this module.





34. Click Save.



#### Test the Integration



At this point, everything is setup. Test the integration!


35.  Go to Security Hub, and open the Findings page. https://console.aws.amazon.com/securityhub 


36. Check the box next to one finding.



37. Under the Actions dropdown, choose Send to Slack.



38. Return to Slack and open the channel you created, #security-hub-alerts. You should see a new message.