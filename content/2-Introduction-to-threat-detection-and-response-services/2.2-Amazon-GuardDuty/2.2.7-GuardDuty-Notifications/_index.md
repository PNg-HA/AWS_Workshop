---
title : "GuardDuty - Notifications"
date : "`r Sys.Date()`"
weight : 7
chapter : false
pre : " <b> 2.2.7 </b> "
---

#### Configure SNS topic

1. Navigate to Amazon SNS. https://console.aws.amazon.com/sns/v3/home 

![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s1.png)
2. Click **Topics** in the left navigation.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s2.png)

3. Click **Create topic**.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s3.png)

4. Select **Standard** type.


5. For the name, enter **"guardduty-findings"**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s5.png)

6. Leave everything else as is and click **Create topic** at the bottom of the page. This will create the topic.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s6.png)
#### Sbscribe to the topic

7. From the **guardduty-findings** topic page, click **Create subscription**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s7.png)

8. On the **Create subscription** page, under **Protocol**, select **email**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s8.png)

9. On the **Create subscription** page, under **Endpoint**, enter your email address that you want to use for this workshop to receive notifications. You can unsubscribe at the end of the workshop.


10. Click **Create subscription**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s10.png)

11. Check the email you entered. Within a couple minutes, you will receive an email at the email address you entered.


12. Confirm the subscription by clicking "Confirm subscription" in the email. This will open a confirmation webpage.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s12.png)

#### Create an EventBridge Rule to send findings to the topic

13. Now that you have subscribed to the SNS topic, you are ready to send findings there. Create an EventBridge rule to listen for events from Security Hub. Navigate to Amazon EventBridge. https://console.aws.amazon.com/events/home
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s13.png)

14. Click the **Create rule** button on the right with "EventBridge Rule" selected.
![VPC](/images/2/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s14.png)

15. On the **Define rule detail** page, name your rule "guardduty-findings". Click **Next**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s15.png)

16. On the **Build event pattern** page, scroll down to the **Event pattern** section, click **Edit pattern** in the bottom right.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s16.png)

17. Add the following event pattern (JSON). This pattern will identify events for Security Hub findings labeled CRITICAL severity.

```js
{
  "source": ["aws.guardduty"],
  "detail-type": ["GuardDuty Finding"],
  "detail": {
    "severity": [
      4, 4.0, 4.1, 4.2, 4.3, 4.4, 4.5, 4.6, 4.7, 4.8, 4.9, 
      5, 5.0, 5.1, 5.2, 5.3, 5.4, 5.5, 5.6, 5.7, 5.8, 5.9, 
      6, 6.0, 6.1, 6.2, 6.3, 6.4, 6.5, 6.6, 6.7, 6.8, 6.9, 
      7, 7.0, 7.1, 7.2, 7.3, 7.4, 7.5, 7.6, 7.7, 7.8, 7.9, 
      8, 8.0, 8.1, 8.2, 8.3, 8.4, 8.5, 8.6, 8.7, 8.8, 8.9
      ]
  }
}
```
18. Click **Next**.


19. On the **Select target(s)** page, from the **Select a target** dropdown, select **SNS topic**.


20. Then from the **Topic** dropdown, select **guardduty-findings**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.7-GuardDuty-Notifications/s20.png)

21. Click **Next**.


22. On the **Configure tags - optional** page, click **Next**.


23. On the **Review and create** page, click **Create rule**. Keep an eye on your email through the rest of the workshop.
