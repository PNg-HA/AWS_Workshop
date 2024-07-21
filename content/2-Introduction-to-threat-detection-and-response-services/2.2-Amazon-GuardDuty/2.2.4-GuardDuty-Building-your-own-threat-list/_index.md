---
title : "GuardDuty - Building your own threat list"
date : "`r Sys.Date()`"
weight : 4
chapter : false
pre : " <b> 2.2.4 </b> "
---

#### Implementing a threat list with a fictitious address


1. Navigate to Amazon S3 in the AWS console.


2. Find the bucket with "**-threatlistbucket-**" in the name and click on the name.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s2.png)

Here is the bucket that the AWS Workshop account has setup
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s2b.png)
3. Upload the threat list you created above to the S3 bucket.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s3.png)


4. Record the S3 URI of the file you have just uploaded. To find the URI, open the bucket, click to open the file, and copy the S3 URI.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s4.png)


5. Now we will add the list your uploaded to the S3 bucket to the Threat lists in GuardDuty. Return to Amazon GuardDuty.


6. In the left navigation of GuardDuty, select **Lists**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s6.png)



7. Click the button **Add a threat IP list**.

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s7.png)

8. In the List name box, give your list a friendly name like "Test Threat List".


9. Under location type the S3 URI for the uploaded file that you recorded earlier.


10. Select **Plaintext** as the Format.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s10.png)


11. Click **I Agree** and then click **Add list**


12. You will need to activate the list once it has been added to GuardDuty. Select the newly added list. Click **Actions**, and then select **Activate**.
![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s12.png)


13. A green status banner will appear saying "Successfully updated the list...".

![VPC](/images/2/2.2-Amazon-GuardDuty/2.2.4-GuardDuty-Building-your-own-threat-list/s13.png)
