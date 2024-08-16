---
title : "Vulnerability management for serverless applications"
date : "`r Sys.Date()`"
weight : 2
chapter : false
pre : " <b> 7.2 </b> "
---

{{%notice info%}}
**Scenario / Problem Statement**: Your team is partnering with an application development team to create a new secure application using AWS Serverless Application Model. Your job is to use Amazon Inspector to detect and remediate vulnerabilities in the application as they are uncovered.
{{%/notice%}}

You will deploy serverless application with AWS Serverless Application Model. The AWS Serverless Application Model (SAM) is an open-source framework for building serverless applications. It provides shorthand syntax to express functions, APIs, databases, and event source mappings.

You'll start by using AWS Cloud9 to clone the application from AWS CodeCommit. AWS Cloud9  is a cloud-based integrated development environment (IDE) that lets you write, run, and debug your code with just a browser. It includes a code editor, debugger, and terminal. Cloud9 comes pre-packaged with essential tools for popular programming languages and the AWS Command Line Interface (CLI) pre-installed so you don’t need to install files or configure your laptop for this workshop.

Setup Cloud9 and deploy your serverless application
Navigate to AWS Cloud9, https://us-east-1.console.aws.amazon.com/cloud9control/home?region=us-east-1 
Click on Create Environment.
Enter "serverless-appsec" in the Name field and leave everything else set the default options.
Click Create at the bottom of the screen.
Wait to see a banner message indicating "Successfully created serverless-appsec...".
Click on the link shown in table to Open the Cloud9 IDE.
You will see a terminal at the bottom of IDE. You may want to expand it to have more working space. You will run CLI commands in this Terminal during the workshop. Keep Cloud9 IDE open in the browser.
Verify that your user is logged in by running the following command in Cloud9 terminal at the bottom of the page.
aws sts get-caller-identity

You should receive a response that includes your Account, UserId, and Arn.

We already have an application in AWS CodeCommit. Optionally, you may want to take a minute and check it out. https://us-east-1.console.aws.amazon.com/codesuite/codecommit/repositories/appsec-serverless-demoapp/browse?region=us-east-1 

Let's clone the existing application to our Cloud9 environment. Run the following command in the terminal window in Cloud9.

git clone https://git-codecommit.us-east-1.amazonaws.com/v1/repos/appsec-serverless-demoapp

Navigate into the application directory by running the below command.


``` cd appsec-serverless-demoapp ```

The sample application is built using AWS SAM. While in the application root directory, use the following command to build the application and wait for it to complete.
sam build --use-container

Build commands

Next, use the deploy command with guided option to specify deployment configuration. It will prompt you several times, as documented in the following instructions.
sam deploy --guided

For the prompt Stack Name [appsec-serverless-demoapp2]: press enter.
For the prompt AWS Region [us-east-1]: press enter.
For the prompt Confirm changes before deploy [Y/n]: type "Y" and press enter.
For the prompt Allow SAM CLI IAM role creation [Y/n]: type "Y" and press enter.
For the prompt Disable rollback [y/N]: type "N" and press enter.
For the prompt HelloWorldFunction has no authentication. Is this okay? [y/N]: type "y" and press enter.
For the prompt LoggingFunction has no authentication. Is this okay? [y/N]: type "y" and press enter..
For the prompt Save arguments to configuration file [Y/n]: type "Y" and press enter.
For the prompt SAM configuration file [samconfig.toml]: press enter.
For the prompt SAM configuration environment [default]: press enter.
SAM Deployment

For the prompt Deploy this changeset? [y/N]: type "y" and press enter.
It will take about 2 minutes to create the stack. It may take several minutes for Amazon Inspector to detect the new functions and generate findings.
Review findings in Amazon Inspector
Open Amazon Inspector in a new tab. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1 

From the navigation panel, open the By Lambda function page under Findings.

Here you should see several Lambda functions with vulnerabilities flagged. Click the name of the function that starts with appsec-serverless-demoapp2-LoggingFunction- to view the Lambda function details. If you don't see it yet, you may need to keep waiting and refresh the page.

Notice there is a finding titled "CWE-117,93 - Log injection". Click the title of the finding to expand the finding details. Here you can see important information, including a description and severity. The vulnerability report also includes the file path, vulnerability location, and suggested remediation, including suggested code change. Note the change you need to make.

Remediate code vulnerability
Return to Cloud9 IDE.
Find the file directory in the navigation on the left. Open the app.py file located in the "logging_sample" directory.
Following the recommended remediation from Inspector. Replace the following line of code:
logger.info("Processing %s", filename)
with this line of code:

logger.info("Processing %s", urllib.parse.quote(filename))

If you get an error, it may be because the recommendation requires a new import. If you get an error, make sure to check your imports and spacing. Make the necessary changes. Your code should look something like:

Fixed code

Save the file, then rebuild the sam application using following command.
sam build --use-container

Once build is completed, deploy the application with the following command and wait for the deployment to complete.
sam deploy --guided

Use the same responses for the guided deployment that you gave on steps 15-24.
For the prompt Deploy this changeset? [y/N]: type "y" and press enter.
Verify the remediation.
Return to the By Lambda function page in Amazon Inspector. https://us-east-1.console.aws.amazon.com/inspector/v2/home?region=us-east-1#/findings/lambda/ 
Again, click the name of the function that starts with appsec-serverless-demoapp2-LoggingFunction- to view the Lambda function details.
Set Finding status next to the Filter criteria to "Show all".
The finding titled "CWE-117,93 - Log injection" should now have a status of Closed. If it does not, wait a couple minutes. Inspector will automatically detect the change and update the finding accordingly.