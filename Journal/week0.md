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


