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

![VPC](/images/3/3.5/s1.png)

![VPC](/images/3/3.5/s1a.png)
2. When you have finished creating your Slack workspace, create a new channel named "security-hub-alerts". This is where we will send notifications from Security Hub using the custom integration. For this exmaple, set the channel visibility to public.
![VPC](/images/3/3.5/s2a.png)
![VPC](/images/3/3.5/s2b.png)

#### Setup Slack application for integration
3. Go to the Slack API site, https://api.slack.com.

![VPC](/images/3/3.5/s3.png)
4. At the top of the page, click Your apps. Then click Create an App.

![VPC](/images/3/3.5/s4.png)
5. In the Create an app popup. choose From scratch.

![VPC](/images/3/3.5/s5.png)

6. For App Name input "security-hub-to-slack". For Pick a workspace to develop your app in select the workspace you just created.


7. Click **Create App**.
![VPC](/images/3/3.5/s7.png)

8. Open **Incoming Webhooks**.
![VPC](/images/3/3.5/s8.png)

9.  On the **Activate Incoming Webhooks** page, switch the toggle to ON.


10. Then at the bottom of the page, click **Add New Webhook to Workspace**.
![VPC](/images/3/3.5/s10.png)

11. On the page that asks, "Where should security-hub-alerts post?" Select **#security-hub-alerts**. Click **Allow**.


12. Back on the Incoming Webhooks page, copy the **Webhook URL** you just created. You will need this later.

![VPC](/images/3/3.5/s12.png)
#### Create Custom Action in Security Hub
Slack is setup. Now you need to configure the Security Hub custom action, EventBridge rule, and Lambda function to push findings to your Slack app (via the webhook).

13. Return to Security Hub. https://console.aws.amazon.com/securityhub/ 


14. Open the Custom actions page from the navigation on the left under Management.


15. Click Create custom action.

![VPC](/images/3/3.5/s15.png)
16. For Action name input "Send to Slack". For Description input "Custom action to send findings to Slack." For Custom action ID input "SendToSlack".


17. Copy the Custom action ARN that was generated for Send to Slack. You will need this later.
#### Create the EventBridge Rule

![VPC](/images/3/3.5/s17.png)
Result:
![VPC](/images/3/3.5/s17b.png)
18. Navigate to **Amazon EventBridge**.



19. Click on the **Create rule** on the right side.


20. In the **Define rule detail** page give your rule a **name** and a **description** that represents the rule's purpose (for example, the same name and description you used for the custom action). Then click **Next**.


21. All Security Hub findings are sent as events to the AWS default event bus. The define pattern section allows you to identify filters to take a specific action when matched events appear. On the **Build event pattern** step, leave the **Event source** set to **AWS events or EventBridge partner events**.

22. Scroll down to Event pattern. Under Event source, leave it set to AWS Services, and under AWS Service, select Security Hub.


23. For the Event Type, choose Security Hub Findings – Custom Action.



24. Then select the Specific custom action ARN(s) radio button and enter the ARN for the custom action that you just created.



25. Click Next.

![VPC](/images/3/3.5/s25.png)

26. On the Select target(s) step, select Lambda function from the Select a target dropdown. Then select security-hub-to-slack from the Function dropdown.

![VPC](/images/3/3.5/s26.png)

27. The Lambda function, security-hub-to-slack, was created using CloudFormation before the start of the lab. If you want to review the code, you can see it in the Lambda console. Click Next.

![VPC](/images/3/3.5/s27.png)
28. On the Configure tags step, click Next.

![VPC](/images/3/3.5/s28.png)

29. On the Review and create step, click Create rule.



#### Customize the Lambda function to send messages to your Slack channel



30. Navigate to the Lambda console and open the Functions page.

![VPC](/images/3/3.5/s30.png)

31. Open the security-hub-to-slack function by clicking on the name.




32. On the function page, open the Configuration tab and then choose Environment variables.

![VPC](/images/3/3.5/s32.png)


33. Click the **Edit** button. Set slackChannel to "security-hub-alerts". Set webHookUrl to the webhook you copied earlier in this module.

![VPC](/images/3/3.5/s33.png)



34. Click Save.



#### Test the Integration



At this point, everything is setup. Test the integration!


35.  Go to Security Hub, and open the Findings page. https://console.aws.amazon.com/securityhub 


36. Check the box next to one finding.

![VPC](/images/3/3.5/s36.png)

37. Under the Actions dropdown, choose Send to Slack.

![VPC](/images/3/3.5/s37.png)
Result:
![VPC](/images/3/3.5/s37b.png)

38. Return to Slack and open the channel you created, #security-hub-alerts. You should see a new message.

See that “security-hub-alerts” channel has news

![VPC](/images/3/3.5/s38.png)
Click on that channel to view:
![VPC](/images/3/3.5/s38b.png)

#### Automate the slack messages

**Problem Statement**: 
Your manager is concerned about the scalability of the appraoch you implemented because it requires you to manually select the findings to be sent to Slack. You have been instructed to automate this so findings that have MEDIUM or HIGH severity labels are automatically sent to Slack via the integration.
Can you figure out what changes need to be made in order to accomplish this objective?

**Hint**: \
6. Select **Specific Severity label(s)** and in the dropdown, pick **MEDIUM** and **HIGH**. That's it. Continue through the same steps as before
![VPC](/images/3/3.5/h6.png)

7. Click **Next**.

8. On the Select target(s) step, select Lambda function from the Select a target dropdown. Then select security-hub-to-slack from the Function dropdown. Click Next.

![VPC](/images/3/3.5/h6-s8.png)

9. On the Configure tags step, click Next.


10. On the Review and create step, click Create rule.
![VPC](/images/3/3.5/h6-s10a.png)
![VPC](/images/3/3.5/h6-s10b.png)