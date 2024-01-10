# How to remove the SSH port when it access from Anywhere using AWS Config? (Automatic Remediation)

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/90e444fe-bb26-4219-b703-c1bcc3e15b83)

## Create IAM Role for Config Automation Assume Role 

Navigate to AWS IAM Console – Select IAM Role 

Create a new Role 

Trusted Identity – AWS Service 

Use case – Use case for other AWS Service – Select – Systems Manager 

Attach permission – Select – AmazonSSMAutomation – Next 

Role Name – AWSConfigSSMRole 

Create a Role 

//note down a Role ARN and will use later. 

## Create SNS Topic with subscriptions 

Refer: SNS demo

#### AWS Config continuously monitors the resources configurations and allows you to automate the evaluation of recorded configurations against desired configurations. 

## Enable AWS Config

AWS Config -> Getting started

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/ece87655-851c-4f0b-9043-a71921fd79c0)

Dont need override settings

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/1031133a-6fd9-4024-830c-97a3abdbc521)

Next -> Next -> Confirm

## Configure AWS Config rule

Go to AWS config in AWS console -> Select add rule 

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/b1e1c972-dce8-4229-a087-429e2d3a82e8)

### Select AWS Managed rules 

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/219ead99-fa21-421e-8953-23e3997309a8)

AWS Manage rules – Select – restricted-ssh 

Config Rules 

Name – restricted-ssh 

Scope of changes – Resources 

Resource type – AWS EC2 Security Groups 

Next – Create Rule

### Remediation

Go to actions and select Manage Remediation. 

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/dd77b1cf-caac-46b7-92dc-f81c91ccde74)

Here you have 2 options: Automatic and Manual. Choose Automatic since we want the inbound rule to get deleted automatically. 

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/3cfd38e1-133f-4546-9e61-52f9d97705b8)

Remediation action list – Select – AWS-SNSPublishNotification (Before you should configure SNS with user mail) 

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/a31b3d83-a42d-42c3-a676-e55c1d5ea9a5)

### Parameter section 

![image](https://github.com/kohlidevops/aws-config-to-notify-when-ssh-openend/assets/100069489/fe15aae1-394d-4c9c-979e-d0fbd42d04ce)

TopicARN, Message and AutomationAssumeRole should place here. 

Then save the changes. 

### Test the scenario 

Open the SSH port in any security groups in particular region. Then you should get mail notification through SNS Service. 
