---
title : "Patching EC2 with Patch Manager"
date : "`r Sys.Date()`"
weight : 1
chapter : false
pre : " <b> 7.1 </b> "
---

1. Find the number of security findings by product in Security Hub by opening the Insights page and clicking into Insight: 23. Top products by counts of findings. https://us-east-1.console.aws.amazon.com/securityhub/home?region=us-east-1#/insights/arn:aws:securityhub:::insight/securityhub/default/28 
![VPC](/images/7/7.1/s1.png)

2. Notice that a significant portion of the security findings in your environment are coming from Inspector.



3. Click Inspector to see the list of individual findings. Based on the title of these findings, it appears most of them, if not all of them, are software vulnerabilities.

![VPC](/images/7/7.1/s3.png)

4. Navigate to the Inspector console. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/dashboard 

![VPC](/images/7/7.1/s4.png)

5. From the Inspector dashboard, you can quickly see that most of your EC2 instances and Lambda functions are being scanned. You can also see the vulnerabilities impacting the most instances and images.


{{%notice info%}}
**Scenario / Problem Statement**: Your team has noticed a growing number of security findings from Amazon Inspector. Your team turned on Inspector to continually scan Amazon Elastic Compute Cloud (EC2), AWS Lambda functions, and container images in Amazon ECR for software vulnerabilities. However, based on the number of findings, it doesn't appear anyone is managing those vulnerabilities. You've been tasked with defining and setting up patch management for your EC2 instances, starting with those being used for development.
{{%/notice%}}

Amazon Inspector is a vulnerability management service that continuously scans your AWS workloads for software vulnerabilities and unintended network exposure. Amazon Inspector automatically discovers and scans running Amazon EC2 instances, container images in Amazon Elastic Container Registry (Amazon ECR), and AWS Lambda functions for known software vulnerabilities and unintended network exposure.

Amazon Inspector creates a finding when it discovers a software vulnerability or network configuration issue. A finding describes the vulnerability, identifies the affected resource, rates the severity of the vulnerability, and provides remediation guidance. You can analyze findings using the Amazon Inspector console, or view and process your findings through other AWS services.

Patch Manager, a capability of AWS Systems Manager, automates the process of patching managed nodes with both security-related updates and other types of updates. You can use Patch Manager to apply patches for both operating systems and applications. (On Windows Server, application support is limited to updates for applications released by Microsoft.) You can use Patch Manager to install Service Packs on Windows nodes and perform minor version upgrades on Linux nodes. You can patch fleets of Amazon Elastic Compute Cloud (Amazon EC2) instances, edge devices, on-premises servers, and virtual machines (VMs) by operating system type.

You can scan instances to see only a report of missing patches, or you can scan and automatically install all missing patches.

Patch Manager uses patch baselines, which include rules for auto-approving patches within days of their release, in addition to optional lists of approved and rejected patches. When a patching operation runs, Patch Manager compares the patches currently applied to a managed node to those that should be applied according to the rules set up in the patch baseline. You can choose for Patch Manager to show you only a report of missing patches (a Scan operation), or you can choose for Patch Manager to automatically install all patches it finds are missing from a managed node (a Scan and install operation).

In this section you will:

- Define patch baseline
- Scan your instances with AWS-RunPatchBaseline via Run Command
- Review patch compliance for managed nodes
- Install missing patches using Run Command
- Schedule patch operations using patch policies
- Review association created by the patch policy

#### Define patch baseline
6. Navigate to Systems Manager and open Patch Manager from the navigation on the left. https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager?region=us-east-1 


7. Click Start with an overview, under the "Create patch policy" button.
![VPC](/images/7/7.1/s7.png)

8. Click the tab, Patch baselines, and then select Create patch baseline.
![VPC](/images/7/7.1/s8.png)

9. On the "Create patch baseline" page, input the Name "AmazonLinux2SecAndNonSecBaseline" in the section Patch baseline details.


10. On the "Create patch baseline" page, select Operating system "Amazon Linux 2" in the section Patch baseline details.



11. In the section Approval rules for operating systems, select "All" Products, "Security" and "Bugfix" Classification, and "Critical" and "Important" Severity. Select Approve patches after a specified number of days and input "14" days. Finally, select "Critical" for Compliance reporting.



12. Confirm that Operating system rule 1 looks like the following image.

![VPC](/images/7/7.1/s12.png)

13. Click Add rule to create "Operating system rule 2".


14. For "Operating system rule 2", leave all the defaults, except set Auto-approval to 7 days, Compliance reporting to "Medium", and check the box for Include nonsecurity updates.



15. Confirm that Operating system rule 2 looks like the following image.

![VPC](/images/7/7.1/s15.png)

16. At the bottom of the page, click Create patch baseline. You should see a banner at the top of the page stating "Create patch baseline request succeeded". This patch baseline will only scan for or install updates based on the criteria defined within the patch approval rules. If an EC2 instance is missing a patch based on the criteria specified within the patch baseline, the instance will be flagged as noncompliant.

![VPC](/images/7/7.1/s16.png)
Result:
![VPC](/images/7/7.1/s16r.png)
#### Scan your instances with AWS-RunPatchBaseline via Run Command
Using Run Command, a capability of AWS Systems Manager, you can remotely and securely manage the configuration of your managed nodes. A managed node is any Amazon Elastic Compute Cloud (Amazon EC2) instance or non-EC2 machine in your hybrid and multi-cloud environment that has been configured for Systems Manager. Run Command allows you to automate common administrative tasks and perform one-time configuration changes at scale.

AWS Systems Manager supports AWS-RunPatchBaseline, a Systems Manager document (SSM document) for Patch Manager, a capability of AWS Systems Manager. This SSM document performs patching operations on managed nodes for both security related and other types of updates. When the document is run, it uses the patch baseline specified as the "default" for an operating system type if no patch group is specified. Otherwise, it uses the patch baseline that is associated with the patch group.

{{%notice tip%}}
You can learn more about the AWS-RunPatchBaseline SSM document under the Documents section of Systems Manager or in the documentation here: https://docs.aws.amazon.com/systems-manager/latest/userguide/patch-manager-aws-runpatchbaseline.html 
{{%/notice%}}

17. Open [Run Command](https://us-east-1.console.aws.amazon.com/systems-manager/run-command?region=us-east-1) from the left navigation.  


18. Click the Run command button.



19. Search for and select the AWS-RunPatchBaseline.
![VPC](/images/7/7.1/s19.png)

20. Under Command parameters, make sure "Scan" is the Operation selected. Leave all the other default parameters.


21. Under Target selection, specify instance tags. Input "Environment" for Tag key and "Development" for Tag value.


22. Click Add next to the key value pair you just entered.
![VPC](/images/7/7.1/s22.png)

23. Under Output options deselect Enable an S3 bucket.

![VPC](/images/7/7.1/s23.png)

24. Click Run at the bottom of the page. Refresh the page until the overall status is "Success".
![VPC](/images/7/7.1/s24.png)

25. Under Targets and outputs, click the Instance ID of one of the EC2 instances targeted (that has the key-value pair you specified) to view the output from the command execution. Take a minute to review the outputs of each step.
![VPC](/images/7/7.1/s25.png)
Observe step 2:
![VPC](/images/7/7.1/s25_step2.png)
Observe step 3:
![VPC](/images/7/7.1/s25_step3.png)
#### Review patch compliance for managed nodes

26. Return to the Patch Manager dashboard using the navigation in Systems Manager. https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager/dashboard?region=us-east-1 
![VPC](/images/7/7.1/s26.png)

27. Here you'll notice that some of your EC2 instances are not compliant and multiple nodes are missing patches.


28. Open the Compliance reporting tab and review the instances listed along with their compliance status and other information, such as missing patches.
![VPC](/images/7/7.1/28.png)

29. Select one of the EC2 instances that has a compliance status of Not compliant.


30. Click the link for that instance in the Security non-compliant count column.
![VPC](/images/7/7.1/s30.png)

31. This page shows the patch summary for the instance. You can optionally select missing patches to see more details.
![VPC](/images/7/7.1/s31.png)

#### Install missing patches using Run Command
32. Return to the Run Command page in Systems Manager and click the button for Run Command.


33. From the list of command documents, search for and select AWS-RunPatchBaseline.


34. This time, under Command parameters, set Operation to "Install".


35. Again, under Target selection, set Tag key to "Environment" and Tag value to "Development". Click Add.
![VPC](/images/7/7.1/s35.png)

36. Expand the Rate control section and set Concurrency targets to 1 and Error threshold to 1.

![VPC](/images/7/7.1/s36.png)

37. Deselect Enable an S3 bucket under Output options.


38. Click the button to Run at the bottom of the page. This will take 3-5 minutes to complete. If any updates are installed by Patch Manager, the patched instance is rebooted by default. Refresh the page until you see "Success".
Observe step 1, 2:
![VPC](/images/7/7.1/s38_step1-2.png)
Observe step 3:
![VPC](/images/7/7.1/s38_step3.png)
39. To check what instances are still missing patches, return to the Patch Manager dashboard and open the Compliance reporting tab. You should see there are no missing patches!


<!-- #### Schedule patch operations using patch policies
A patch policy is a configuration you set up using Quick Setup, a capability of AWS Systems Manager. Patch policies provide more extensive and more centralized control over your patching operations than is available with other methods of configuring patching. A patch policy defines the schedule and baseline to use when automatically patching your nodes and applications.

Instead of using other methods of patching your nodes, use a patch policy to take advantage of these major features:

- Single setup – Setting up patching operations using a maintenance window or State Manager association can require multiple tasks in different parts of the Systems Manager console. Using a patch policy, all your patching operations can be set up in a single wizard.
- Multi-account/Multi-Region support – Using a maintenance window, a State Manager association, or the Patch now feature in Patch Manager, you're limited to targeting managed nodes in a single AWS account-AWS Region pair. If you use multiple accounts and multiple Regions, your setup and maintenance tasks can require a great deal of time, because you must perform setup tasks in each account-Region pair. However, if you use AWS Organizations, you can set up one patch policy that applies to all your managed nodes in all AWS Regions in all your AWS accounts. Or, if you choose, a patch policy can apply to only some organizational units (OUs) in the accounts and Regions you choose. A patch policy can also apply to a single local account, if you choose.
- Installation support at the organizational level – The existing Host Management configuration option in Quick Setup provides support for a daily scan of your managed nodes for patch compliance. However, this scan is done at a predetermined time and results in patch compliance information only. No patch installations are performed. Using a patch policy, you can specify different schedules for scanning and installing. You can also choose the frequency and time of these operations by using custom CRON or Rate expressions. For example, you could scan for missing patches every day to provide you with regularly updated compliance information. Your installation schedule could be just once a week to avoid unwanted downtime.
- Simplified patch baseline selection – Patch policies still incorporate patch baselines, and there are no changes to the way patch baselines are configured. However, when you create or update a patch policy, you can select the AWS managed or custom baseline you want to use for each operating system (OS) type in a single list. It’s not necessary to specify the default baseline for each OS type in separate tasks.


40. Return to the Patch Manager dashboard using the navigation in Systems Manager. https://us-east-1.console.aws.amazon.com/systems-manager/patch-manager/dashboard?region=us-east-1 


41. Click Create patch policy.
{{%notice tip%}}
If the AWS Quick Setup is displayed prompting you to Get started with Quick Setup, select us-east-1 as the home region and click the Get started button.
{{%/notice%}}


42.	On the Create patch policy page, set the Configuration name to "patch-policy-workshop".


43.	For Scanning and installation, select Scan and install.


44.	For Installation schedule, select Custom install schedule.

45.	For Installation frequency, select Custom CRON Expression and then input the CRON expression "cron(30 23 ? * SAT *)" to install updates on Saturday at 23:30 UTC.


46.	Make sure Wait to install updates until first CRON interval. is selected.


47.	Under Patch baseline, select Custom patch baseline.



48.	Under View or change baselines, for "Amazon Linux 2" select "AmazonLinux2SecAndNonSecBaseline".


49.	Deselect Write output to S3 bucket.


50.	Under Targets, select Specify node tag.


51.	Below "Instance tag details", click Add. Input "Environment" for Key and "Development" for Value.


52.	For Rate control, input "50" Percentage for both Concurrency and Error threshold.


53.	Check the box to Add required IAM policies to existing instance profiles attached to your instances.


54.	Click Create. The Patch Manager Quick Setup will take approximately 2 minutes to update. Wait for the banner to display "Your Patch Manager Quick Setup configuration was successfully updated."


#### Review association created by the patch policy
55. Open State Manager using the navigation on the left in Systems Manager. https://us-east-1.console.aws.amazon.com/systems-manager/state-manager?region=us-east-1# 


56. From the list of Associations, find the association starting with "AWS-QuickSetup-PatchPolicy-ScanForPatches-LA-" Click the link for association id to open the description.


57. If the Status is "Failed" or "Pending", click Apply association now to run the patch operation now. If prompted with "Are you sure you want to send the request to apply this association now?" click Apply again.


58. Wait 2-3 minutes for the status to show "Success".


59. Open the association starting with "AWS-QuickSetup-PatchPolicy-ScanForPatches-LA-" again. Review the information under each tab, starting with Description and ending with Execution history. -->