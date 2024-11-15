# Setting-up-a-CI-CD-pipeline-on-AWS-with-Jenkins-and-CodeDeploy
 Setting Up AWS Resources:

Create an IAM user with programmatic access and record credentials.
Create an S3 bucket and record the bucket name and URL.
Launch an EC2 instance (preferably with AWS Linux AMI) and record the instance ID.
2. Creating a CodeBuild Project:

In the AWS Developer Tools console, create a CodeBuild project with these configurations:
Choose a name and record it.
Select "No source" (source will be handled by Jenkins).
Choose a managed environment with Amazon Linux OS.
Create a service role and record its name.
Specify your S3 bucket name and create a build artifact name (e.g., codebuild-artifact.zip).
Choose "None" for artifacts packaging.
3. Creating a CodeDeploy Configuration:

In the AWS console under "Deploy" -> "Deployment Configurations," create a configuration with:
A descriptive name.
EC2 selected as the compute platform.
Optionally enable cloud logging for debugging.
4. Configuring Jenkins:

Install the File Operation, AWS CodeBuild, and HTTP Request plugins.
5. Creating a Jenkins Job:

Set up a Jenkins job with these steps:
Build Step: AWS CodeBuild:
Provide your IAM access key credentials.
Enter your CodeBuild project name, region, and select "Use Jenkins Source."
Build Step: File Operations (Delete):
Delete all files (*) to clean the build environment.
Build Step: HTTP Request:
Construct the S3 URL with your bucket name and build artifact name.
Ignore SSL errors and specify the correct region.
Set advanced options: Connection Timeout (0), Expected Response Codes (100-399), Output Response to File (codebuild-artifact.zip).
Build Step: File Operations (Unzip & Delete):
Unzip the downloaded codebuild-artifact.zip.
Delete the codebuild-artifact.zip file.
Post-build Step: Deploy to AWS CodeDeploy:
Enter the information saved from CodeBuild and CodeDeploy configurations.
