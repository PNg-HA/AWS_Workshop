---
title : "Detective - Investigations"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.4.4 </b> "
---

#### Ientify a resource to investigate

1. Open the **IAM console** and navigate to the **Roles** page. https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles

 
2. Search for the role starting with "**cfn-GuardDutyLabsInfrastructure-GeneralInstanceRole-**".



3. Click the role name.
![VPC](/images/2/2.4/2.4.4/s3.png)


4. Copy the ARN from the top of the page. You will need this in a later step.
![VPC](/images/2/2.4/2.4.4/s4.png)
#### Running a detective investigation

5. Open the **Investigations** page from the **Detective** console. https://us-east-1.console.aws.amazon.com/detective/home?region=us-east-1#investigations 


6. In the Investigations page, choose **Run investigation** in the top right corner.
![VPC](/images/2/2.4/2.4.4/s6.png)

7. In the Select resource section, you have three ways to run an investigation. You can choose to run the investigation for a resource recommended by Detective. You can run the investigation for a specific resource. You can also investigate a resource from the Detective Search page. Select **Specify an AWS role or user with an ARN**.



8. Under **Select resource type** select **AWS role**.


9. Under **Resource ARN**, input the ARN of the resource you identified above.



10. Click **Run investigation**.
![VPC](/images/2/2.4/2.4.4/s11.png)


11. Wait for the investigation to complete. It should display a status of "Successful" within 2-3 minutes.
![VPC](/images/2/2.4/2.4.4/s12a.png)



12. Click the ID of the investigation to review the results. Depending on how long this account has been provisioned, you may see "We did not observe uncommon behavior...". However, if something has been detected, you will see the report contains an Indicators of Compromise section that includes details regarding one or more indicators of compromise. Investigations summary highlights anomalous indicators that require attention, for the selected scope time. Using the summary, you can more quickly identify the root cause of potential security issues, identify patterns, and understand the resources impacted by security events.

![VPC](/images/2/2.4/2.4.4/s12b.png)
![VPC](/images/2/2.4/2.4.4/s12c.png)

If you want to find more detail about the indicator type **IP address**, you will see different TTPs:
![VPC](/images/2/2.4/2.4.4/s12d.png)

Indicators type TTP:
![VPC](/images/2/2.4/2.4.4/s12e.png)