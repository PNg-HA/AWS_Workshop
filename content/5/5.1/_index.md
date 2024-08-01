---
title : "Setting up notifications"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 5.1 </b> "
---
After Security Hub has ingested a finding identifying a threat or stating that a configuration that needs attention, the next step is take action and resolve the finding. With Amazon EventBridge, you can automate your AWS services to respond automatically to system events such as application availability issues or resource changes.

Amazon EventBridge is a serverless event bus that makes it easier to build event-driven applications at scale using events generated from AWS services. Events from AWS services are delivered to EventBridge in near-real time and on a guaranteed basis. You can write simple rules to indicate which events you are interested in and what automated actions to take when an event matches a rule. The actions that can be automatically triggered include the following:
+ Invoking an AWS Lambda function
+ Activating an AWS Step Functions state machine
+ Notifying an Amazon SNS topic or an Amazon SQS queue
+ Sending a finding to a third-party ticketing, chat, SIEM, or incident response and management tool

Security Hub automatically sends all new findings and all updates to existing findings to EventBridge as EventBridge events. You can also create custom actions that allow you to send selected findings and insight results to EventBridge. You then configure EventBridge rules to respond to each type of event.

In this module, we will create an EventBridge rule to automate notifications on a filtered list of findings from Security Hub. We will configure Amazon Simple Notification Service so we will receive email alerts when we receive a HIGH or CRITICAL severity finding in Security Hub. Amazon Simple Notification Service (Amazon SNS) is a fully managed messaging service for both application-to-application (A2A) and application-to-person (A2P) communication.

In a real-world scenario, this particular example would not scale, but it will demonstrate how AWS Security Hub works with Amazon EventBridge.

#### Configure Amazon Simple Notification Service topic

1. Navigate to [Amazon SNS](https://console.aws.amazon.com/sns/v3/home).


2. Click Topics in the left navigation.


3. Click Create topic.


4. Select Standard type.


5. For the name, enter "high-severity-security-hub-findings". Result:


6. Leave everything else as is and click Create topic at the bottom of the page. This will create the topic.
![VPC](/images/5/5.1/s6.png)

#### Subscribe to the topic

7. From the high-severity-security-hub-findings topic page, click Create subscription.


8. On the Create subscription page, under Protocol, select Email.



9. On the Create subscription page, under Endpoint, enter your email address that you want to use for this workshop to receive notifications. You can unsubscribe at the end of the workshop.


10. Click Create subscription.
![VPC](/images/5/5.1/s10.png)


11. Within a couple minutes, you will receive an email at the email address you entered. Confirm the subscription by clicking Confirm subscription in the email.


#### Create an EventBridge Rule to send findings to the topic
Now that we have subscribed to our SNS topic, we are ready to send findings to the topic. To do this, we will create an EventBridge rule that matches Security Hub events for HIGH and CRITICAL severity findings.

12. Navigate to Amazon EventBridge. https://console.aws.amazon.com/events/home 


13. Click Create rule.


14. On the Define rule detail page, name your rule "high-severity-security-hub-findings".


15. Click Next.


16. On the Build event pattern page, in the Event pattern section, click Edit pattern.


17. Input the following Event Pattern:
![VPC](/images/5/5.1/s17.png)

```
{
  "source": ["aws.securityhub"],
  "detail": {
    "findings": {
      "Severity": {
        "Label": ["HIGH", "CRITICAL"]
      }
    }
  }
}
```


18. Click Next.


19. On the Select target(s) page, from the Select a target dropdown, select SNS topic.


20. Then from the Topic dropdown, select high-severity-security-hub-findings.
![VPC](/images/5/5.1/s20.png)

21. Click Next.


22. On the Configure tags - optional page, click Next.


23. On the Review and create page, click Create rule.
![VPC](/images/5/5.1/s23.png)

#### Test the notifications
The EventBridge rule we configured will push a notification on any update or new Security Hub finding that have a severity label of "HIGH" or "CRITICAL". We could wait for a new or updated finding, but it will be faster to test this by updating a finding ourselves. An easy way to do this is by changing the Workflow status of a finding.

For findings, the workflow status tracks the progress of your investigation into a finding. The workflow status is specific to an individual finding. It does not affect the generation of new findings.

24. Return to [Security Hub](https://console.aws.amazon.com/securityhub). 


25. Open the Findings page.



26. Add a new filter, for Severity label is HIGH. This is case sensitive. Click Apply.
![VPC](/images/5/5.1/s23.png)


27. Click the title for any of the high severity findings to open it.



28. Under the Workflow status dropdown in the finding details, change the workflow from New to Notified.
![VPC](/images/5/5.1/s28.png)


29. You will see a banner display "Successfully changed workflow status". This update will result in you receiving an email containing the JSON for this finding. You will want to unsubscribe from these notifications before completing the workshop.
![VPC](/images/5/5.1/s29.png)

Notification:
![VPC](/images/5/5.1/s29b.png)
#### Challenge
At this point you understand most of the basics of how AWS Security Hub works with Amazon EventBridge. Let's put that knowledge to the test.

After configuring the notification for all HIGH and CRITICAL findings, you realized that you are getting too many notifications between resource compliance changes, the resolution of findings, and co-workers changing the workflow status of findings.

Using what you learned so far, try to modify the notification you set up to only email you findings with a HIGH severity label, that have a workflow status of New, and that Security Hub receives from GuardDuty (findings must match all 3 conditions). Amazon GuardDuty is a threat detection service that continuously monitors your AWS accounts and workloads for malicious activity and delivers detailed security findings for visibility and remediation. These are alerts you want to act on quickly.

![VPC](/images/5/5.1/c1.png)