---
title: "Blog 2"
date: 2026-07-08
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---


# [Data Caching Across Microservices in a Serverless Architecture](https://aws.amazon.com/blogs/architecture/data-caching-across-microservices-in-a-serverless-architecture/)

Organizations are increasingly migrating from **monolithic architectures** to **microservices** to improve scalability, accelerate development, and deploy new features independently.

However, as applications are divided into multiple microservices, a new challenge emerges: each service often needs to retrieve data from multiple sources such as databases, legacy systems, or shared services. As a result, every request may require several backend calls, increasing latency and placing additional load on source systems.

To address this challenge, AWS recommends deploying a **data cache** close to the microservices layer. This reduces the number of direct backend requests, improving application performance while lowering response latency.

---

## Why Do We Need a Data Cache?

A cache is a high-speed storage layer used to temporarily store frequently accessed data.

Instead of querying the database or source system for every request, a microservice first attempts to retrieve data from the cache. Since cached data is typically stored in memory, it can be accessed much faster than querying backend systems directly.

Using a cache provides several benefits:

- Reduces request latency.
- Minimizes direct access to databases and backend systems.
- Decreases backend load during traffic spikes.
- Improves the scalability of the overall system.

Depending on application requirements and data characteristics, AWS introduces two different caching strategies.

---

## Use Case 1 – On-Demand Caching (Cache-Aside Pattern)

![Blog2](/images/3-BlogsTranslated/Blog2_1.jpg)

> *Figure 1. Reducing latency by caching frequently accessed data.*

In the first scenario, AWS implements the **Cache-Aside Pattern**, also known as **Lazy Loading**.

The concept is simple: data is added to the cache only when it is requested.

The workflow is as follows:

1. The Billing Service receives a client request.
2. The service checks the cache using the object key.
3. If the data exists (**Cache Hit**), it is returned immediately.
4. If the data is not found (**Cache Miss**), the Billing Service retrieves it from the **System of Record**.
5. The retrieved data is stored in the cache with a configured **Time-To-Live (TTL)**.
6. Subsequent requests read directly from the cache instead of querying the backend.

With this approach, only frequently accessed data is cached, significantly reducing the number of backend queries.

---

## How AWS Implements Use Case 1

![Blog2](/images/3-BlogsTranslated/Blog2_2.jpg)

> *Figure 2. AWS services used to implement the Cache-Aside Pattern.*

To implement this architecture in a serverless environment, AWS uses the following services:

- **Amazon API Gateway** receives client requests.
- **AWS Lambda** hosts microservices such as Billing, Payment, and Profile.
- **Amazon ElastiCache** provides an in-memory caching layer.
- **Amazon EventBridge** distributes events between microservices.
- **Cache Manager** updates or invalidates cached data whenever necessary.

When data changes—for example, after a successful payment—the Payment Service publishes an event to EventBridge. The Cache Manager processes this event to update or invalidate the corresponding cache entry, preventing clients from receiving stale data.

---

## Limitations of the Cache-Aside Pattern

Although the Cache-Aside Pattern significantly reduces backend requests, it has an important limitation.

If data in the source system is updated while the cached copy has not yet expired, users may still receive outdated information until the cache's TTL expires.

For applications such as payment systems, banking, or customer management platforms, serving stale data may lead to business issues.

To address this limitation, AWS introduces a second strategy.

---

## Use Case 2 – Event-Driven Cache Synchronization

![Blog2](/images/3-BlogsTranslated/Blog2_3.jpg)

> *Figure 3. Proactively populating and synchronizing the cache through events.*

Unlike the Cache-Aside Pattern, this approach does not wait for the first request before populating the cache.

Instead, data is proactively loaded into the cache through automated processes such as **Initial Load** or **Change Data Capture (CDC)**.

Whenever data changes in the source system:

1. The change is detected through the CDC pipeline.
2. An event is published to the event store.
3. The Cache Manager receives the event.
4. The cache is immediately updated or invalidated.
5. Microservices always read synchronized data directly from the cache.

As a result, microservices rarely need to perform real-time queries against backend systems while still serving up-to-date data.

---

## How AWS Implements Use Case 2

![Blog2](/images/3-BlogsTranslated/Blog2_4.jpg)

> *Figure 4. AWS services used for proactive cache synchronization.*

For this architecture, AWS replaces the caching layer with components better suited for larger datasets.

The main services include:

- **Amazon API Gateway** receives client requests.
- **AWS Lambda** runs the microservices.
- **Amazon DynamoDB** stores cached data persistently.
- **Amazon DynamoDB Accelerator (DAX)** accelerates read operations through in-memory caching.
- **Amazon EventBridge** distributes data change events.
- **Cache Manager** synchronizes cached data in near real time.

Compared to the first approach, this architecture is better suited for applications with large datasets or workloads that require continuously updated information.

---

## When Should You Use Each Strategy?

AWS does not recommend a single caching strategy for every application.

The **Cache-Aside Pattern** is ideal when:

- Data is read frequently.
- Data changes infrequently.
- Simplicity and rapid implementation are priorities.

In contrast, an **Event-Driven Cache** is more suitable when:

- Data changes frequently.
- Strong data consistency is required.
- The application already follows an **Event-Driven Architecture**.
- Large datasets make real-time backend queries impractical.

---

## Conclusion

Data caching not only improves application performance but also significantly reduces the load on backend systems in a microservices architecture.

As demonstrated by these two AWS approaches, each caching strategy addresses different application requirements. The **Cache-Aside Pattern** is simple and easy to implement, while the **Event-Driven Cache** is better suited for systems that require highly consistent, frequently updated data under heavy workloads.

The key takeaway is not **whether to use caching**, but **choosing the caching strategy that best fits your application's characteristics and business requirements**.
