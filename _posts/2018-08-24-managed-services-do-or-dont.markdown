---
layout: post
title: "Managed Services: Do or Don't"
date: 2018-08-24
category: blog
tags: cloud aws programming
---

In this post I'll try to distill a few concerns I have with the constant push from the cloud providers in providing more and more managed cloud services. It's great So with managed services I mean platform as a service that is providing all or close to all of the maintenance from you.

Yesterday I visited AWS Summit in Anaheim in August and I can summarize almost every talk with AWS will do even more heavy lifting for you and you have to do serverless.

Looking at your typical web application almost all of the heavy work can be done using serverless microservices (AWS Lambda together with API Gateway), fully managed database (AWS DynamoDb), handling of logs and alarms (Cloudwatch), user authentication (AWS Cognito). And if you are a mobile developer and have used the Backend for Frontend (BFF) pattern, AWS will manage that for you as well with GraphQL-server (AWS Appsync).

My experience, from just a few years ago, is that in order to accomplish anything interesting you would need to provision your own compute resource (various EC2 instances) where some would run the business logic and some would run the database. And you would need think hard on how to set up the networking (VPC) to make it secure but also accessible. In addition, user authentication and management would be a fairly complicated feature you would need to implement in your backend and handle in your own database. So what would take a team to do back then can now be done by a single person or a much smaller team now.

So the question is, are more managed services and the serverless paradigm all good? The downside is the dreaded vendor lock-in since you are now more heavily relying on cloud vendor's specialized solution.  

I've spent quite a few years doing various kinds of embedded systems. These systems can have a very long life cycle. When you have something out in the field for a long time, it is crucial to know that you have either source code and tools to maintain the solution or the third-party pieces you are relying on will be supported for the life of the product. For these kinds of products, I could see myself using something equivalent to the embedded version of fully managed solution but I would carefully analyze the long term commitment to the technology from the vendor.
Back to your typical web application. I do think going serverless and use fully managed services is the future. If you are about to implement a large solution based on managed services, I would go the extra mile to make sure I architect loosely coupled with clean interfaces between the code that is specific to your application and the code that is tightly coupled with your cloud vendor's API.
