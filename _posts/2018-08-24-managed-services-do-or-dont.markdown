---
layout: post
title: "Managed Services: Do or Don't"
date: 2018-08-24
category: blog
tags: cloud aws programming
---

In this post, I'll try to distill a few concerns I have with the constant push from the cloud providers to provide more and more managed cloud services.

With managed services I mean platform as a service where the cloud provider is there is no need for you to allocate specific infrastructure like virtual servers. The cloud provider is providing all or close to all of the maintenance for you.

Yesterday I visited AWS Summit in Anaheim in and I can summarize almost every talk with that everyone should go serverless and let AWS manage more of your burden.

Looking at your typical web application almost all of the heavy work can be done now be using serverless microservices (AWS Lambda together with API Gateway), fully managed database (AWS DynamoDb), managed services for handling of logs and alarms (AWS Cloudwatch), and a service for user authentication (AWS Cognito). And if you are a mobile developer and have used the Backend for Frontend (BFF) pattern, AWS will now simplify and manage that for you as well with a GraphQL server (AWS Appsync).

This is in stark contrast to just a few years ago. Back then, in order to accomplish anything interesting you would need to provision your own compute resource (various EC2 instances) where some would run the webserver, others would run some business logic and some would run the database. And you would need think hard on how to set up the networking between the nodes to make it secure but also accessible. In addition, user authentication and management would be a fairly complicated feature you would need to implement in your back-end and handle in your own database. So what would take a team to do back then can now be done by a single person or a much smaller team now.

So the question is, are more managed services and the serverless paradigm all good?

The downside is the dreaded vendor lock-in since you are now more heavily relying on cloud vendor's specialized solution.  

I've spent quite a few years doing various kinds of embedded systems. These systems can have a very long life cycle. When you have something out in the field for a long time, it is crucial to know that you have either the source code and tools to maintain the solution or knowledge that the third-party libraries/frameworks you are relying on will be supported for the life of the product. For these kinds of products, I could see myself using something equivalent to the embedded version of fully managed solution but I would carefully analyze the long term commitment to the technology from the vendor.

Back to your typical web application. I do think going serverless and use fully managed services is the future. However, if you are about to implement a large solution based on managed services, I would go the extra mile to make sure I architect a loosely coupled system with clean interfaces between the code that is specific to my application and the code that is tightly coupled with your cloud vendor's API. This way if the cloud vendor's API changed or are no longer supported your migration to another solution will at least be doable. 
