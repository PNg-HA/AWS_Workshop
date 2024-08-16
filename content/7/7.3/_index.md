---
title : "Integrating Amazon Inspector into a CI/CD pipeline"
date : "`r Sys.Date()`"
weight : 3
chapter : false
pre : " <b> 7.3 </b> "
---

In this module, you will learn how to integrate Amazon Inspector container image scans directly into your CI/CD pipeline to scan for software vulnerabilities, and provide reports at the end of your build. This feature allows customers to investigate, and remediate, risks before deployment.

The Amazon Inspector CI/CD integration utilizes a combination of the Amazon Inspector SBOM Generator, and the Amazon Inspector Scan API, to produce vulnerability reports for your container images. The Amazon Inspector SBOM Generator creates a software bill of materials (SBOM) from a provided container image, then, the Amazon Inspector Scan API scans that SBOM and creates a report with details on any vulnerabilities detected.

You can achieve a CI/CD integration with Amazon Inspector through the Amazon Inspector plugins purposefully built for individual CI/CD solutions and available in their marketplace, or you can create your own custom scanning integration.

Use the Jenkins plugin to integrate Amazon Inspector into your CI/CD pipeline. You will use images that are located in Docker Hub.

#### Install Jenkins on an EC2 instance
In this section, you will install Jenkins and the Amazon Inspector SBOM Generator on an EC2 instance. Jenkins is an open-source automation server that integrates with a number of AWS Services, including: AWS CodeCommit, AWS CodeDeploy, Amazon EC2 Spot, and Amazon EC2 Fleet. The Amazon Inspector SBOM Generator (Sbomgen) is a binary tool that produces a software bill of materials (SBOM) for a container image. An SBOM is a collected inventory of the software installed on a system. Sbomgen works by scanning for files known to contain information about installed packages. If one of these files is found, the tool extracts package names, versions, and other metadata.

1. Navigate to Amazon EC2 console and open the Instances page. https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running 



2. Select the Jenkins instance, and click the Connect button at the top of the page.



3. Switch to the Session Manager tab on the Connect to instance page.



4. Click Connect. This will open a Systems Manager session.



5. Download the Jenkins repo.
```sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo ```

6. Import a key file from Jenkins to enable installation.
```sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key```


7. Upgrade any Jenkins components.
```sudo yum upgrade```


8. Install Java.
```sudo yum install java-11-amazon-corretto -y```



9. Install Jenkins.
```sudo yum install jenkins -y```


10. Enable the Jenkins service to start at boot.
```sudo systemctl enable jenkins```


11. Start Jenkins as a service.
```sudo systemctl start jenkins```



12.  Verify the Jenkins service is running.
```sudo systemctl status jenkins```


13. Click Terminate.

14.	Return to the Instances page in the [EC2 console](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running;tag:Name=Jenkins).  


15.	Select the Jenkins instance.


16.	Go to the Security tab of the instance details.


17.	Note the IAM Role under Security details. You will look up the full ARN of this role later in this module.


18.	Under Security groups, click the security group attached to the instance. It will have a name starting with "sg-".


19.	You will be brought to the security group's configuration. Click the Edit inbound rules button.


20.	You will need to connect to the EC2 instance over port 8080 to reach the admin page of Jenkins. Click the Add rule button.


21.	For the new rule, set Type to Custom TCP. Set Port range to "8080". For Source, select My IP.


22.	Click Save rules.


#### Configure an IAM role for the Jenkins CI/CD integration
23. We need to create an IAM role that allows access to the Amazon Scan API, that scans the software bill of materials, in Jenkins. Open the IAM console. https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/home 



24. From the navigation pane of the IAM console, open Policies.



25. Click Create Policy.


26. In Policy Editor click the JSON button, and replace the policy with the following:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": "inspector-scan:ScanSbom",
            "Resource": "*"
        }
    ]
}
```

27.	Click Next.


28.	On the Review and create page, input "InspectorCICDscan-policy" for the Policy name. This policy will be attached to the role you’ll create in a few steps.


29.	Click Create policy.


30.	From the navigation pane of the IAM console, open Roles and search for the role you noted earlier (attached to the EC2 instance, Jenkins). The role should start with "cfn-InspectorLabsInfrastructure" and includes the term "SSMInstanceRole". Click the name of the role to open the details.


31.	Copy the ARN of the role. You will need this in a few steps.


32.	From the navigation pane of the IAM console, open Roles and then click Create Role.


33.	For Trusted entity type, select Custom trust policy.


34.	Replace the Custom trust policy with the following. Replace "ARN" with the ARN of the IAM role you copied a few steps previously (that includes the phrase "SSMInstanceRole").
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "ARN"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```


35. Click Next.



36. In Add permissions search for and select the policy you created earlier, "InspectorCICDscan-policy". Then click Next.


37. Give the role the name "InspectorCICDscan-role". Then click Create Role.


{{%notice tip%}}
This IAM permission setup is used for the workshop, but may not be applicable to your own environment. In the workshop, the role attached to the EC2 instance that is hosting Jenkins, is given trust to the newly created InspectorCICDscan-role. The InspectorCICDscan-role will be used by Jenkins. Therefore, when Jenkins uses the InspectorCICDscan-role, it will assume the IAM role attached to the EC2 instance, which has permissions to the Inspector Scan service.
{{%/notice%}}

#### Configure Jenkins
38. Now that the instance has the correct security group and AWS permissions, you can configure Jenkins to use Inspector to scan for vulnerabilities. Return to the EC2 console, and open [Instances page](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running;tag:Name=Jenkins). 



39. Select the Jenkins instance.


40. Click the Connect button at the top of the page.


41. Under the Session Manager tab, select Connect.



42. We will need to get the initial administrator password to log into Jenkins.
```sudo cat /var/lib/jenkins/secrets/initialAdminPassword```


43. Copy the result. This is the initial Jenkins admin password. You will need this in a later step.


44. Click Terminate.



45. Go back to the EC2 console, and select the [Jenkins instance](https://us-east-1.console.aws.amazon.com/ec2/home?region=us-east-1#Instances:instanceState=running;tag:Name=Jenkins).  



46. Under the Details tab, copy the public IP address under Public IPv4 address.

47. In a new browser tab, paste the IP address and add ":8080" to it. It should look similar to "xx.xx.xx.xx:8080".


48. Input the initial admin password from earlier, and click Continue.


49. From the Customize Jenkins page, click Install suggested plugins.


50. Once the installation is complete, the Create First Admin User page will open. Input your information, and then select Save and Continue.


51. On the Instance Configuration page, click Save and Finish.



52. On the page displaying Jenkins is ready, click Start using Jenkins.



53. From the navigation, open Manage Jenkins.


54. Under System Configuration, open Plugins.



55. Select Available plugins from the menu.



56. Search available plugins for Amazon Inspector Scanner.



57. Select the checkbox next to Amazon Inspector Scanner, and then click Install.



58. Wait for the installation to complete. At the bottom of the page, click Go back to the top page.


59. From the navigation, click Manage Jenkins.


60. Under System Configuration, click Nodes.



61. Click on Built-In Node.


62. Open Script Console from the navigation.


63. In the text box add the following line:
```System.setProperty("hudson.model.DirectoryBrowserSupport.CSP", "")```

64. Click Run.


#### Build a Jenkins job
65.	Go to the Jenkins dashboard using the link in the top left, and select Create a job.


66.	Under Enter an item name input "InspectorScan".

67.	Select Freestyle project, and then click OK.


68.	Scroll down to the Build Steps section, click the dropdown Add build step, and choose Amazon Inspector Scan.


69.	Select Automatic for the Inspector-sbomgen Installation Method, and then choose Linux, AMD64.


70.	For Image Id, input "centos:latest".


71.	For AWS Region, select us-east-1.

72.	In a new tab, open the IAM console and search for the role you created earlier named InspectorCICDscan-role. Copy the ARN. https://us-east-1.console.aws.amazon.com/iam/home?region=us-east-1#/roles/details/InspectorCICDscan-role 


73.	Paste the ARN of your IAM role into the Jenkins build configuration page.


74.	Click Save.


75.	This will bring you to a dashboard for your Jenkins build job. From the navigation, click Build Now to create your first build. If you run into an issue with the build, try clicking Build Now again or ask your facilitator for assistance.

#### Review the build for vulnerabilities
76. Under Build History, look for a green checkmark to indicate a successful build. Click on the green checkmark.


77. This will bring you to the console output of the Jenkins job. You can see that Inspector scanned the image and provides links to reports. Click the link next to Build Artifacts.


78. Click index.html. If there are any vulnerabilities, they will show in the report.