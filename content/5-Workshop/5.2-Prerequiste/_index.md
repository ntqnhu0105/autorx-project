---
title : "Prerequisites"
date : 2026-07-09
weight : 2
chapter : false
pre : " <b> 5.2 </b> "
---

In this section, you will prepare your environment before deploying the AutoRx system on AWS.

Before you begin, make sure you have the following:

- An AWS account or an AWS Academy Learner Lab account.
- An IAM user with sufficient permissions to deploy the services used in this workshop.
- The AutoRx project source code.
- Git and Visual Studio Code installed on your computer.

This workshop uses the **Asia Pacific (Singapore)** Region (`ap-southeast-1`) to minimize latency and ensure that all required AWS services are available.

### IAM Permissions

Attach the following IAM permission policy to your IAM user to deploy and clean up the resources created in this workshop.

![IAM](/images/5-Workshop/5.2-Prerequisite/iam-permission.jpg)

### Select the AWS Region

In the upper-right corner of the AWS Management Console, select the following Region:

```text
Asia Pacific (Singapore)
ap-southeast-1
```

This Region will be used throughout the workshop.

![region](/images/5-Workshop/5.2-Prerequisite/region.jpg)