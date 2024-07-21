---
title : "Cross-region finding aggregation"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 3.4. </b> "
---
With cross-Region aggregation, you can aggregate findings, finding updates, insights, control compliance statuses, and security scores from multiple Regions to a single aggregation Region. You can then manage all of this data from the aggregation Region.

Suppose you set US East (N. Virginia) as an aggregation Region, and US West (Oregon) and US West (N. California) as your linked Regions. When you view the Findings page in US East (N. Virginia), you see the findings from all three Regions. Updates to those findings are also reflected in all three Regions. The enablement status of a control must be modified in each Region. If a control is enabled in a linked Region but disabled in the aggregation Region, you can see the compliance status of the control from the aggregation Region, but you cannot enable or disable that control from the aggregation Region.

#### Set up cross-region finding aggregation
1. You may have already seen a prompt to **Configure finding aggregation**. You can click any of them to proceed with the setup. If you didn't notice this, open the **Regions** page under Settings from the navigation on the left.



2. Click **Configure finding aggregation**.
![VPC](/images/3/3.4/s2.png)


3. Select **US East (N. Virginia) - us-east-1** as your Aggregation Region.



4. Select all of the available regions.



6. Click **Save** at the bottom of the page. This will enable cross-region aggregation of findings, but will not actually turn on Security Hub in those other regions.
![VPC](/images/3/3.4/s6.png)

**Note to Participants**: When you click Save to enable cross-region aggregation, it will erase the aggregated scores and control data in Security Hub. It does not remove findings or stop generating findings, but you will notice that the scores will not appear (on the Summary page, for example) until new scores are calculated based on multiple regions. You may want to do the section of the workshop titled "Security Posture Management" before completing this module.


#### Turn on the AWS security services you use in the linked Region
Although many AWS security services were already enabled in us-east-1 (N. Virginia) and you set up finding aggregation in Security Hub, that does not mean all of those services are turned on in the other regions you plan to operate out of. To quickly enable all the same AWS security services in us-east-2 (Ohio), use CloudFormation to deploy everything in the second Region. As soon as those services are enabled in the second region, findings from them with automatically be aggregated in Security Hub to us-east-1 (N. Virginia) thanks to the setup you already did above.

7. Open the https://us-east-1.console.aws.amazon.com/cloudformation/home?region=us-east-1#/stacks/?filteringText=cfn&filteringStatus=active&viewNested=false page in the CloudFormation console.
![VPC](/images/3/3.4/s7.png)

8. Click the name of the stack, **cfn**.
![VPC](/images/3/3.4/s8.png)


9. Open the **Outputs** tab.
![VPC](/images/3/3.4/s9.png)


10. Click the "SecurityServicesCloudFormationLink" to launch a preconfigured CloudFormation template in us-east-2 (Ohio). This will open the "Quick create stack" page in us-east-2 (Ohio) and populate the stack information and parameters. Make sure it says "Ohio" in top right of the page.



11. Leave the values as is for "Stack name", "MyAssetsBucketName", and "MyAssetsBucketPrefix". If there are no values present, ask your facilitator.
![VPC](/images/3/3.4/s11.png)


12. On the bottom of the page, check the boxes next to **I acknowledge that AWS CloudFormation might create IAM resources with custom names and I acknowledge that AWS CloudFormation might require the following capability: CAPABILITY_AUTO_EXPAND**.



13. Click Create stack. This will enable AWS Config, Security Hub, Inspector, Detective, Macie, and GuardDuty in us-east-2 (Ohio). Because you already configured finding aggregation in Security Hub, as soon as findings from any of these services appear in Security Hub in this region, they will also appear in your aggregation region. This will take a few minutes. You may continue without waiting.
![VPC](/images/3/3.4/s13.png)


14. Return to Security Hub. If you did not switch back to the original workshop region, you will see a prompt to Go to aggregation region. Click that prompt. Make sure you are back in us-east-1 (N. Virginia) (you should see "N. Virginia" in the top right corner of your screen).



15. Once you have returned to Security Hub in your aggregation region, you'll notice that the Findings page now includes findings from multiple regions, and the visualizations on the Summary page also highlight cross-region findings. We'll further explore those pages later.

(After 5 minute)
![VPC](/images/3/3.4/s15.png)