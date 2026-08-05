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
​Triggers an email alert when actual spend crosses 80% of the threshold.

{```json
  {
    "Notification": {
      "ComparisonOperator": "GREATER_THAN",
      "NotificationType": "ACTUAL",
      "Threshold": 80,
      "ThresholdType": "PERCENTAGE"
    },
    "Subscribers": [
      {
        "Address": "kkwe******@gmail.com",
        "SubscriptionType": "EMAIL"
      }
    ]
  }
]
}


![A screenshot of the notification in my github workspace](<img width="1366" height="768" alt="notification_in_CLI" src="https://github.com/user-attachments/assets/c2bd4804-a59a-47a1-a3bc-08ef59236dbd" />
)


##Budget Policy (aws/budget.json)
Sets up a monthly cost limit capped at $1 USD.

```json
{
    "BudgetLimit": {
        "Amount": "1",
        "Unit": "USD"
    },
    "BudgetName": "Example Tag Budget",
    "BudgetType": "COST",
    "CostFilters": {
        "TagKeyValue": [
            "user:Key$value1",
            "user:Key$value2"
        ]
    },
    "CostTypes": {
        "IncludeCredit": true,
        "IncludeDiscount": true,
        "IncludeOtherSubscription": true,
        "IncludeRecurring": true,
        "IncludeRefund": true,
        "IncludeSubscription": true,
        "IncludeSupport": true,
        "IncludeTax": true,
        "IncludeUpfront": true,
        "UseBlended": false
    },
    "TimePeriod": {
        "Start": 1477958399,
        "End": 3706473600
    },
    "TimeUnit": "MONTHLY"
}

![Also a screenshot of budget created](<img width="1366" height="768" alt="proof of budget" src="https://github.com/user-attachments/assets/ec8e9a83-577c-4e32-ae84-07072e995508" />
)




##CLI Deployment Command
# Fetch active AWS Account ID and deploy
export ACCOUNT_ID=$(aws sts get-caller-identity --query "Account" --output text)

aws budgets create-budget \
  --account-id $ACCOUNT_ID \
  --budget file://aws/budget.json \
  --notifications-with-subscribers file://aws/notifications-with-subscribers.json


  
Install AWS CLI

I also run these commands indivually to perform the install manually
Create a new User and Generate AWS Credentials

    Go to (IAM Users Console]  create a new user
    Enable console access for the user
    Create a new Admin Group and apply AdministratorAccess
    Create the user and go find and click into the user
    Click on Security Credentials and Create Access Key
    Choose AWS CLI Access
    Download the CSV with the credentials

Set Env Vars

 I set these credentials for the current bash terminal

export AWS_ACCESS_KEY_ID=""
export AWS_SECRET_ACCESS_KEY=""
export AWS_DEFAULT_REGION=us-east-1

I tell Github to remember these credentials if we relaunch our workspaces
By creating a secret in the repo to store the data

 env AWS_ACCESS_KEY_ID=""
 env AWS_SECRET_ACCESS_KEY=""
 env AWS_DEFAULT_REGION=us-north-1

Check that the AWS CLI is working and you are the expected user

$aws sts get-caller-identity (INPUT)
{
    "UserId": "AIDAXS56W3MLUDP5JLWAG",
    "Account": "5217016******",
    "Arn": "arn:aws:iam::5217016*****:user/General-Reborn"
}







