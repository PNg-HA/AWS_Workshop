---
title : "Security Hub - Notifications"
date : "`r Sys.Date()`"
weight : 5
chapter : false
pre : " <b> 2.1.5 </b> "
---

#### Configure SNS topic

1. Navigate to Amazon SNS. https://console.aws.amazon.com/sns/v3/home 

![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s1.png)
2. Click **Topics** in the left navigation.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s2.png)

3. Click **Create topic**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s3.png)

4. Select **Standard** type.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s4.png)

5. For the name, enter **"security-hub-findings"**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s5.png)

6. Leave everything else as is and click **Create topic** at the bottom of the page. This will create the topic.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s6.png)
#### Sbscribe to the topic

7. From the **security-hub-findings** topic page, click **Create subscription**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s7.png)

8. On the **Create subscription** page, under **Protocol**, select **email**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s8.png)

9. On the **Create subscription** page, under **Endpoint**, enter your email address that you want to use for this workshop to receive notifications. You can unsubscribe at the end of the workshop.


10. Click **Create subscription**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s10.png)

11. Check the email you entered. Within a couple minutes, you will receive an email at the email address you entered.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s11.png)

12. Confirm the subscription by clicking "Confirm subscription" in the email. This will open a confirmation webpage.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s12.png)

#### Create an EventBridge Rule to send findings to the topic

13. Now that you have subscribed to the SNS topic, you are ready to send findings there. Create an EventBridge rule to listen for events from Security Hub. Navigate to Amazon EventBridge. https://console.aws.amazon.com/events/home
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s13.png)

14. Click the **Create rule** button on the right with "EventBridge Rule" selected.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s14.png)

15. On the **Define rule detail** page, name your rule "security-hub-findings". Click **Next**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s15.png)

16. On the **Build event pattern** page, scroll down to the **Event pattern** section, click **Edit pattern** in the bottom right.


17. Add the following event pattern (JSON). This pattern will identify events for Security Hub findings labeled CRITICAL severity.

```js
{
  "source": [
    "aws.securityhub"
  ],
  "detail-type": [
    "Security Hub Findings - Imported"
  ],
  "detail": {
    "findings": {
      "ProductName": [
        "Security Hub"
      ],
      "Severity": {
        "Label": [
          "CRITICAL"
        ]
      }
    }
  }
}
```
18. Click **Next**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s18.png)

19. On the **Select target(s)** page, from the **Select a target** dropdown, select **SNS topic**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s19.png)

20. Then from the **Topic** dropdown, select **security-hub-findings**.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s20.png)

21. Click **Next**.


22. On the **Configure tags - optional** page, click **Next**.


23. On the **Review and create** page, click **Create rule**. Keep an eye on your email through the rest of the workshop.
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s23.png)

View the created rule:
![VPC](/images/2-Introduction-to-threat-detection-and-response-services/2.1-AWS-Security-Hub/2.1.5-Security-Hub-Notifications/s24.png)