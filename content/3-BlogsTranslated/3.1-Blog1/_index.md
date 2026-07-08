---
title: "Blog 1"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---


# [Latency-Based Routing: Leveraging Amazon CloudFront for a Multi-Region Active-Active Architecture](https://aws.amazon.com/vi/blogs/networking-and-content-delivery/latency-based-routing-leveraging-amazon-cloudfront-for-a-multi-region-active-active-architecture/)

Modern applications are increasingly deployed across multiple AWS Regions to improve availability and reduce latency for users around the world. However, deploying backend services in multiple Regions is only the first step. A greater challenge is ensuring that users are always connected to the most appropriate Region without changing the application's endpoint.

Amazon CloudFront delivers content closer to users through its global network of Edge Locations. However, when the CloudFront origin is an **Amazon API Gateway Custom Domain**, direct support for **Latency-Based Routing** is not available.

In this article, AWS introduces an architecture that combines **Amazon CloudFront**, **Amazon Route 53**, **Amazon API Gateway**, and **AWS Certificate Manager (ACM)** to build a **Multi-Region Active-Active** solution. This approach allows CloudFront to continue using a single origin endpoint, while Route 53 automatically routes requests to the Region with the lowest latency.

---

## Overall Architecture

To implement this architecture, AWS introduces an additional DNS routing layer behind CloudFront instead of allowing CloudFront to connect directly to the API Gateway endpoint in each Region.

CloudFront communicates with a single origin domain, while Route 53 uses **Latency-Based Routing** to determine which Region should handle each request based on network latency.

![Blog1](/images/3-BlogsTranslated/Blog1_1.jpg)

> *Figure 1. Multi-Region Active-Active architecture using CloudFront, Route 53, and API Gateway Custom Domains.*

---

## DNS Configuration

The key to this solution lies in its DNS design.

Instead of configuring CloudFront to point directly to API Gateway endpoints in individual Regions, AWS uses two separate domain names:

- The primary domain (`example.com`) is configured in Route 53 and points to the CloudFront distribution.
- The origin domain (`api.example.com`) is used by CloudFront to access backend services.

For `api.example.com`, Route 53 creates multiple **Latency-Based Routing** records, with each record pointing to an API Gateway Custom Domain deployed in a different AWS Region.

In addition, every API Gateway Custom Domain is configured with its own TLS certificate using **AWS Certificate Manager (ACM)** to ensure secure HTTPS communication between CloudFront and the backend services.

This DNS architecture makes it easy to add or remove Regions without affecting end users.

---

## Request Flow

Once the DNS configuration is complete, requests are processed through the following sequence.

![Blog1](/images/3-BlogsTranslated/Blog1_2.jpg)

> *Figure 2. Request routing from users to the Region with the lowest latency.*

**Step 1:** A user accesses the application domain (`example.com`).

**Step 2:** Route 53 returns the address of the CloudFront distribution.

**Step 3:** The request is routed to the nearest CloudFront Edge Location.

**Step 4:** CloudFront queries Route 53 using the origin domain (`api.example.com`).

**Step 5:** Route 53 applies **Latency-Based Routing** to select the API Gateway Custom Domain in the Region with the lowest latency, and the request is forwarded to the appropriate backend service for processing.

This entire routing process is automatic and completely transparent to end users.

---

## Deployment Considerations

Although this architecture provides significant benefits, there are several important considerations:

- Each AWS Region must have its own API Gateway deployment and Custom Domain.
- Every Custom Domain requires a TLS certificate issued through **AWS Certificate Manager (ACM)**.
- Route 53 acts as the central routing layer between Regions.
- CloudFront is responsible only for receiving client requests and forwarding them to the origin domain—it does not determine which Region processes the request.
- Data synchronization across Regions must be designed separately based on the application's requirements.

---

## Benefits of the Architecture

Adding Route 53 as a routing layer behind CloudFront overcomes the limitations of API Gateway Custom Domains in Multi-Region deployments.

This architecture offers several advantages:

- Users are automatically routed to the AWS Region with the lowest latency.
- The application remains available even if an entire Region becomes unavailable.
- A single public domain serves the entire application.
- The architecture can be easily expanded to additional AWS Regions in the future.
- CloudFront's global Edge Location network helps reduce response times and improve the user experience.