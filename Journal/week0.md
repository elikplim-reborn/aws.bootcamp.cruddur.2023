# Week 0 - Architecture & Billing

## 🏗️ Architecture Blueprint
Here is the multi-tier web architecture I designed using Lucidchart.

![Architecture Blueprint]<https://lucid.app/lucidchart/fed30c09-1653-49eb-a1df-70c57f20a80d/edit?viewport_loc=-1506%2C-1354%2C1851%2C965%2C0_0&invitationId=inv_4d214ec4-66ba-4412-96b1-8f05484cc69f> 

<img width="1366" height="768" alt="Screenshot (14)" src="https://github.com/user-attachments/assets/a82661e9-6a02-4a29-94c3-70c031f9fa78" />


## ☁️ AWS Setup & Billing
This week, I set up my AWS Management Console and configured foundational Identity and Access Management (IAM) settings for secure access control. I also familiarized myself with AWS billing tools to monitor usage and avoid unexpected costs.##

##Automating AWS Cost Limits via CLI
​Automating AWS Cost Management by defining budget policies in JSON and deploying them directly via the AWS CLI inside GitHub Codespaces.

​Configuration Files 

##Notification Rules (aws/notifications-with-subscribers.json)
​Triggers an email alert when actual spend crosses 80% of the threshold.[
  {
    "Notification": {
      "ComparisonOperator": "GREATER_THAN",
      "NotificationType": "ACTUAL",
      "Threshold": 80,
      "ThresholdType": "PERCENTAGE"
    },
    "Subscribers": [
      {
        "Address": "kkweku713@gmail.com",
        "SubscriptionType": "EMAIL"
      }
    ]
  }
]

##Budget Policy (aws/budget.json)
Sets up a monthly cost limit capped at $1 USD.
{
  "BudgetLimit": {
    "Amount": "1",
    "Unit": "USD"
  },
  "BudgetName": "Example Tag Budget",
  "BudgetType": "COST",
  "TimeUnit": "MONTHLY"
}


##CLI Deployment Command
# Fetch active AWS Account ID and deploy
export ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text)

aws budgets create-budget \
  --account-id $ACCOUNT_ID \
  --budget file://aws/budget.json \
  --notifications-with-subscribers file://aws/notifications-with-subscribers.json


  
Install AWS CLI

We'll also run these commands indivually to perform the install manually
Create a new User and Generate AWS Credentials

    Go to (IAM Users Console]  create a new user
    Enable console access for the user
    Create a new Admin Group and apply AdministratorAccess
    Create the user and go find and click into the user
    Click on Security Credentials and Create Access Key
    Choose AWS CLI Access
    Download the CSV with the credentials

Set Env Vars

We will set these credentials for the current bash terminal

export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1

We'll tell Github to remember these credentials if we relaunch our workspaces
By creating a secrest in the repo to store the data

 env AWS_ACCESS_KEY_ID=""
 env AWS_SECRET_ACCESS_KEY=""
 env AWS_DEFAULT_REGION=us-east-1

Check that the AWS CLI is working and you are the expected user

aws sts get-caller-identity

You should see something like this:

{
    "UserId": "AIFBZRJIQN2ONP4ET4EK4",
    "Account": "655602346534",
    "Arn": "arn:aws:iam::655602346534:user/andrewcloudcamp"
}

Enable Billing

We need to turn on Billing Alerts to recieve alerts...

    In your Root Account go to the Billing Page
    Under Billing Preferences Choose Receive Billing Alerts
    Save Preferences

Creating a Billing Alarm
Create SNS Topic

    We need an SNS topic before we create an alarm.
    The SNS topic is what will delivery us an alert when we get overbilled
    aws sns create-topic

We'll create a SNS Topic

aws sns create-topic --name billing-alarm

which will return a TopicARN

We'll create a subscription supply the TopicARN and our Email

aws sns subscribe \
    --topic-arn TopicARN \
    --protocol email \
    --notification-endpoint your@email.com

Check your email and confirm the subscription
Create Alarm

    aws cloudwatch put-metric-alarm
    Create an Alarm via AWS CLI
    We need to update the configuration json script with the TopicARN we generated earlier
    We are just a json file because --metrics is is required for expressions and so its easier to us a JSON file.

aws cloudwatch put-metric-alarm --cli-input-json file://aws/json/alarm_config.json

Create an AWS Budget

aws budgets create-budget

Get your AWS Account ID

aws sts get-caller-identity --query Account --output text

    Supply your AWS Account ID
    Update the json files
    This is another case with AWS CLI its just much easier to json files due to lots of nested json

aws budgets create-budget \
    --account-id AccountID \
    --budget file://aws/json/budget.json \
    --notifications-with-subscribers file://aws/json/budget-notifications-with-subscribers.json




