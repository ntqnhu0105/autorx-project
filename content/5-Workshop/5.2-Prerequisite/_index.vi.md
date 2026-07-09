---
title: "Chuẩn bị (Prerequisite)"
date: 2026-07-09
weight: 2
chapter: false
pre: " <b> 5.2 </b> "
---

# Chuẩn bị môi trường

Trong phần này, chúng ta sẽ chuẩn bị các công cụ và tài nguyên cần thiết trước khi bắt đầu triển khai hệ thống **Smart Vehicle Management** trên AWS.

#### 1. Tài khoản AWS
* Cần có một tài khoản AWS đang hoạt động.
* Khuyến nghị chọn Region (Khu vực) là **ap-southeast-1 (Singapore)** để các dịch vụ hoạt động ổn định nhất trong phạm vi bài lab.

**IAM user - workshop admin** có những policy sau:

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




#### 2. Cài đặt các công cụ phát triển (Local Tools)
Để có thể chạy trình giả lập xe (Vehicle Simulator) và tùy chỉnh code, máy tính của bạn cần cài đặt sẵn:
* **Node.js** (Phiên bản 18.x hoặc 20.x trở lên) và trình quản lý gói `npm`.
* **Trình soạn thảo mã (Code Editor):** Khuyến nghị sử dụng [Visual Studio Code](https://code.visualstudio.com/).
* **AWS CLI:** Công cụ dòng lệnh của AWS. Bạn cần chạy lệnh `aws configure` để cấu hình *Access Key*, *Secret Key* và *Default Region* trước khi bắt đầu. (Bạn đã có AWS CLI trên máy nên chỉ cần kiểm tra lại phần này).

#### 3. Mã nguồn dự án (Source Code)
* Trích xuất mã nguồn dự án về máy tính local (bao gồm frontend Next.js, backend nội bộ, và công cụ Vehicle Simulator).
* Cài đặt các thư viện cần thiết bằng lệnh:
  ```bash
  npm install
  ```
