---
title: "Prerequisites"
date: 2026-07-09
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

# Prepare the Environment

In this section, you will prepare the required tools and AWS resources before deploying the **Smart Vehicle Management** system on AWS.

## 1. AWS Account

Before you begin, make sure you have:

- An active AWS account.
- It is recommended to use **Asia Pacific (Singapore)** (`ap-southeast-1`) as the AWS Region to ensure that all services used in this workshop are fully supported.

* IAM User **workshop-admin** should have the following IAM permission policy:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "DynamoDBPermissions",
            "Effect": "Allow",
            "Action": [
                "dynamodb:CreateTable",
                "dynamodb:DeleteTable",
                "dynamodb:DescribeTable",
                "dynamodb:UpdateTable",
                "dynamodb:PutItem",
                "dynamodb:GetItem",
                "dynamodb:Query",
                "dynamodb:Scan"
            ],
            "Resource": "*"
        },
        {
            "Sid": "LambdaPermissions",
            "Effect": "Allow",
            "Action": [
                "lambda:CreateFunction",
                "lambda:DeleteFunction",
                "lambda:GetFunction",
                "lambda:GetFunctionConfiguration",
                "lambda:UpdateFunctionCode",
                "lambda:UpdateFunctionConfiguration",
                "lambda:InvokeFunction",
                "lambda:AddPermission",
                "lambda:RemovePermission"
            ],
            "Resource": "*"
        },
        {
            "Sid": "ApiGatewayPermissions",
            "Effect": "Allow",
            "Action": [
                "apigateway:GET",
                "apigateway:POST",
                "apigateway:PUT",
                "apigateway:PATCH",
                "apigateway:DELETE"
            ],
            "Resource": "arn:aws:apigateway:*::/*"
        },
        {
            "Sid": "IoTPermissions",
            "Effect": "Allow",
            "Action": [
                "iot:CreateThing",
                "iot:DeleteThing",
                "iot:DescribeThing",
                "iot:CreateKeysAndCertificate",
                "iot:DeleteCertificate",
                "iot:UpdateCertificate",
                "iot:AttachPrincipalPolicy",
                "iot:AttachThingPrincipal",
                "iot:DetachPrincipalPolicy",
                "iot:DetachThingPrincipal",
                "iot:CreatePolicy",
                "iot:DeletePolicy",
                "iot:CreateTopicRule",
                "iot:DeleteTopicRule",
                "iot:ReplaceTopicRule",
                "iot:DescribeEndpoint"
            ],
            "Resource": "*"
        },
        {
            "Sid": "CloudWatchAndSNSPermissions",
            "Effect": "Allow",
            "Action": [
                "logs:CreateLogGroup",
                "logs:CreateLogStream",
                "logs:PutLogEvents",
                "logs:DescribeLogGroups",
                "cloudwatch:PutMetricAlarm",
                "cloudwatch:DeleteAlarms",
                "cloudwatch:DescribeAlarms",
                "sns:CreateTopic",
                "sns:DeleteTopic",
                "sns:Subscribe",
                "sns:ListSubscriptionsByTopic"
            ],
            "Resource": "*"
        },
        {
            "Sid": "IAMPermissions",
            "Effect": "Allow",
            "Action": [
                "iam:CreateRole",
                "iam:DeleteRole",
                "iam:GetRole",
                "iam:CreatePolicy",
                "iam:DeletePolicy",
                "iam:AttachRolePolicy",
                "iam:DetachRolePolicy",
                "iam:PutRolePolicy",
                "iam:DeleteRolePolicy",
                "iam:PassRole"
            ],
            "Resource": "*"
        }
    ]
}

```

## 2. Install the Development Tools

To run the **Vehicle Simulator** and modify the project source code, install the following tools on your local machine:

- **Node.js** (version **18.x**, **20.x**, or later) with **npm**.
- **Code Editor:** We recommend using **Visual Studio Code**.
- **AWS CLI:** Install the AWS Command Line Interface and configure it by running:

```bash
aws configure
```

Provide the following information:

- AWS Access Key ID
- AWS Secret Access Key
- Default AWS Region
- Default output format

If AWS CLI is already installed, simply verify that your configuration is correct before continuing.

## 3. Project Source Code

Download or extract the **Smart Vehicle Management** project to your local machine.

The project includes:

- Next.js frontend
- Backend services
- Vehicle Simulator

Install all required dependencies by running:

```bash
npm install
```