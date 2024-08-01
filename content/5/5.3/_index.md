---
title : "Automated Security Response on AWS"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 5.3 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: At the scale of your company, you have realized that there are thousands of misconfigured resources. You need to find a solution to centrally remediating miconfiguations automatically, at scale.
{{%/notice%}}

{{%notice info%}}
In this workshop, only one account is used so this solution does not show all its features. For the whole scenario of this solution, please review my own deploytment at [this GitHub repo](https://github.com/PNg-HA/CSPM-with-AWS-Security-Hub).
{{%/notice%}}

![S3](/images/5/5.3/automated-security-response-on-aws.png)


#### Deploy Automated Security Response on AWS via CloudFormation
{{%notice warning%}}
The ASR solution requires deployment of 3 CloudFormation templates. You can view templates [here](https://docs.aws.amazon.com/solutions/latest/automated-security-response-on-aws/solution-overview.html). You may launch all of the stacks at the same time. You do not need to wait for one stack to complete before starting the next. The 3 stacks take 10 minutes to complete. The stacks will also launch multiple nested stacks to create all the necessary resources.
{{%/notice%}}

1. Deploy the admin stack by clicking the Deploy Automated Security Response admin stack admin stack button below. Make sure you are launching the template in the region you have been working in. To launch this solution in a different AWS Region, use the Region selector in the AWS Management Console navigation bar.
[Deploy Automated Security Response admin stack admin stack](https://console.aws.amazon.com/cloudformation/home?#/stacks/create/review?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Faws-security-hub-automated-response-and-remediation%2Flatest%2Faws-sharr-deploy.template&stackName=aws-security-hub-automated-response-and-remediation-admin&param_LoadAFSBPAdminStack=no&param_LoadCIS120AdminStack=no&param_LoadCIS140AdminStack=no&param_LoadNIST80053AdminStack=no&param_LoadPCI321AdminStack=no&param_LoadSCAdminStack=yes&ReuseOrchestratorLogGroup=no)


2. Review the stack name and the parameters for the template. The parameters are pre-populated, but confirm that the parameters selected match the screenshot below. Scroll to the bottom of the **Quick create stack** screen and check the box for **I acknowledge that AWS CloudFormation might create IAM resources** and the box for **I acknowledge that AWS CloudFormation might require the following capability**: **CAPABILITY_AUTO_EXPAND**.

![VPC](/images/5/5.3/s2.png)
![VPC](/images/5/5.3/s2b.png)
3. Click **Create stack**. You may continue without waiting for the stack to complete.

4. Deploy the member roles stack by clicking the **Deploy ASR member roles stack** button below. Make sure you are launching the template in the region you have been working in. To launch this solution in a different AWS Region, use the Region selector in the AWS Management Console navigation bar. [Deploy ASR member roles stack](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Faws-security-hub-automated-response-and-remediation%2Flatest%2Faws-sharr-member.template&stackName=aws-security-hub-automated-response-and-remediation-member&param_LogGroupName=SHARR-Log-Group&param_LoadAFSBPMemberStack=no&param_LoadCIS120MemberStack=no&param_LoadCIS140MemberStack=no&param_LoadNIST80053MemberStack=no&param_LoadPCI321MemberStack=no&param_LoadSCMemberStack=yes)

5. Review the stack name and the parameters for the template. For the SecHubAdminAccount parameter enter the account ID you see in the top right of the AWS Console.


6. Scroll to the bottom of the **Quick create stack** screen and check the box for **I acknowledge that AWS CloudFormation might create IAM resources**.


7. Click **Create stack**.

8. Deploy the member runbooks stack by clicking the Deploy ASR member runbooks stack button below. Make sure you are launching the template in the region you have been working in. To launch this solution in a different AWS Region, use the Region selector in the AWS Management Console navigation bar.
Deploy ASR member runbooks stack: [Deploy ASR member runbooks stack](https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/quickcreate?templateURL=https:%2F%2Fs3.amazonaws.com%2Fsolutions-reference%2Faws-security-hub-automated-response-and-remediation%2Flatest%2Faws-sharr-member.template&stackName=aws-security-hub-automated-response-and-remediation-member&param_LogGroupName=SHARR-Log-Group&param_LoadAFSBPMemberStack=no&param_LoadCIS120MemberStack=no&param_LoadCIS140MemberStack=no&param_LoadNIST80053MemberStack=no&param_LoadPCI321MemberStack=no&param_LoadSCMemberStack=yes).



9. Review the stack name and the parameters for the template. The parameters are pre-populated, but confirm that the parameters selected match the screenshot below. For the SecHubAdminAccount parameter enter the account ID you see in the top right of the AWS Console again.
![VPC](/images/5/5.3/s9.png)

10. Scroll to the bottom of the Quick create stack screen and check the box for **I acknowledge that AWS CloudFormation might create IAM resources** and the box for **I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**.


11. Click **Create stack**.


12. At this point, you must wait for all of the CloudFormation stacks to complete deploying. You may check progress here: https://console.aws.amazon.com/cloudformation/home?#/stacks 
![VPC](/images/5/5.3/s12.png)

#### Leverage Automated Security Response on AWS for remediation
This solution includes the playbook remediations for the security standards defined as part of the Center for Internet Security (CIS) AWS Foundations Benchmark v1.2.0, Center for Internet Security (CIS) AWS Foundations Benchmark v1.4.0, AWS Foundation Security Best Practices (AFSBP) v.1.0.0, Payment Card Industry Data Security Standard (PCI-DSS) v3.2.1, and Security Control (SC) v2.0.0. You can learn more here: https://docs.aws.amazon.com/solutions/latest/automated-security-response-on-aws/playbooks-1.html 

13. Let's try out the solution. Return to [Security Hub](https://console.aws.amazon.com/securityhub).


14. Open the Controls page.


15. Filter for the control **EC2.2**.
![VPC](/images/5/5.3/s15.png)

16. Click the name **The VPC default security group should not allow inbound and outbound traffic**.
![VPC](/images/5/5.3/s16.png)

17. Select the checkbox to select all failed checks.


18. Click the **Actions** drop down and select **Remediate with ASR**. Choosing this action sends a copy of the finding(s) to EventBridge, kicking off the automation to change the default security group rules setting to restrict inbound and outbound traffic.

19. After a few minutes the checks should have a status of RESOLVED.


{{%notice warning%}}
It may take about 3 to 5 minutes to run and update. After that, wait and refresh the page again.
{{%/notice%}}

