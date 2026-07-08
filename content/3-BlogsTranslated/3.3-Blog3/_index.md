---
title: "Blog 3"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---


# [Optimizing Your AWS Infrastructure for Sustainability – Part III: Networking](https://aws.amazon.com/blogs/architecture/optimizing-your-aws-infrastructure-for-sustainability-part-iii-networking/)

When optimizing infrastructure on AWS, many people focus on selecting the right EC2 instance type, tuning databases, or reducing storage usage.

However, another critical factor that directly impacts performance, cost, and resource consumption is **networking**.

Every user request must travel across the network. The farther the data travels and the larger the payload, the more network resources are consumed. This not only increases latency but also raises **data transfer costs**.

In the third installment of the **Optimizing Your AWS Infrastructure for Sustainability** series, AWS focuses on optimizing the networking layer and introduces principles and services that help reduce data transfer, improve performance, and build more sustainable cloud architectures.

---

## Why Optimize Networking?

Unlike compute or storage, the networking layer often receives less attention during system design.

However, every interaction between users and applications relies on the network. As user traffic grows and datasets become larger, network resource consumption increases accordingly.

According to AWS, optimizing networking provides several benefits:

- Reduces latency.
- Minimizes data transfer.
- Improves the user experience.
- Lowers operational costs.

More importantly, efficient networking makes better use of infrastructure resources, contributing to a more sustainable cloud environment.

---

## AWS Recommends Two Networking Optimization Strategies

According to AWS, network optimization should focus on two primary goals.

### 1. Reduce the Distance Data Travels

Bring data as close to users as possible to minimize transmission time.

Examples include:

- Choosing the AWS Region closest to the majority of users.
- Deploying workloads across multiple Regions for global applications.
- Using a Content Delivery Network (CDN) to distribute content.

### 2. Reduce the Amount of Data Transferred

In addition to shortening transmission distance, AWS recommends minimizing the volume of data sent across the network by:

- Compressing data using **Gzip** or **Brotli**.
- Returning only the data that users actually need.
- Caching static content.
- Optimizing API payload sizes.

Together, these two principles significantly reduce the network resources required for every request.

---

## Bringing Content Closer to Users with Amazon CloudFront

![Blog3](/images/3-BlogsTranslated/Blog3_1.jpg)

> *Figure 1. Comparing direct access to Amazon S3 with access through Amazon CloudFront.*

The first diagram illustrates two different approaches for delivering content.

On the left, users around the world send requests directly to an **Amazon S3 bucket** located in a single AWS Region.

This means:

- Requests travel much longer distances.
- The origin server handles all traffic.
- Latency increases for users located far from the Region.
- Data transfer costs become higher.

On the right, AWS introduces **Amazon CloudFront**.

CloudFront stores cached copies of content at **Edge Locations** and **Regional Edge Caches** distributed around the world.

When a user sends a request:

- CloudFront first checks whether the content is already available in the edge cache.
- If the content is cached, it is delivered immediately from the nearest location.
- Only cache misses require CloudFront to retrieve data from the origin.

This approach provides several advantages:

- Reduces the distance data travels.
- Lowers latency.
- Reduces the number of requests reaching the origin.
- Saves bandwidth and data transfer costs.
- Improves application scalability.

This is why CloudFront is not only a CDN, but also a valuable service for improving both application performance and infrastructure sustainability.

---

## Monitoring Your Infrastructure to Identify Optimization Opportunities

![Blog3](/images/3-BlogsTranslated/Blog3_2.jpg)

> *Figure 2. Amazon CloudWatch and AWS Trusted Advisor help monitor infrastructure components.*

After optimizing the network, the next question becomes:

**How do you know whether your infrastructure is truly optimized?**

AWS recommends combining **Amazon CloudWatch** and **AWS Trusted Advisor** to continuously monitor and evaluate the entire infrastructure.

In this architecture:

- Compute services such as **Amazon EC2**, **Amazon ECS**, and **Amazon EMR** publish operational metrics to CloudWatch.
- Storage services including **Amazon S3**, **Amazon EBS**, **Amazon EFS**, and **Amazon FSx** provide metrics related to traffic and resource utilization.
- **Amazon CloudFront** reports metrics such as **Cache Hit Ratio**, traffic volume, and latency.

CloudWatch collects and visualizes all of these metrics.

Meanwhile, **AWS Trusted Advisor** analyzes the collected data and provides recommendations, such as:

- Whether CloudFront should be used in front of an S3 bucket.
- Which buckets generate excessive direct data transfer.
- Which infrastructure components can be optimized to reduce network traffic.
- Opportunities to lower costs while improving performance.

Instead of relying on assumptions, organizations can make optimization decisions based on real operational metrics.

---

## Additional AWS Recommendations

Beyond using CloudFront, AWS also recommends several best practices for reducing network traffic:

- Select the AWS Region closest to the majority of users.
- Use multiple Regions for globally distributed applications.
- Compress data with **Gzip** or **Brotli** before transmission.
- Reduce API payload sizes.
- Cache API responses whenever appropriate.
- Continuously monitor network metrics using CloudWatch.
- Use Trusted Advisor to identify optimization opportunities.

Although each optimization may seem small individually, together they can significantly improve performance, reduce costs, and lower infrastructure resource consumption.

---

## Conclusion

Networking is much more than the communication layer between users and applications—it directly affects system performance, operational costs, and scalability.

Through this article, AWS emphasizes that network optimization is not simply about reducing latency. It is about making more efficient use of network resources by bringing data closer to users, minimizing data transfer, and continuously monitoring infrastructure metrics.

These practices are fundamental to building AWS architectures that are **high-performing, cost-efficient, and sustainable** over the long term.