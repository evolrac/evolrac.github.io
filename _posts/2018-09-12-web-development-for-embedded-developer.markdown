---
layout: post
title: "Web development for Embedded developers"
date: 2018-09-12
category: blog
tags: web programming
---

> "Any one can do web development!"

I've spent quite a few years of my career working together with embedded developers and I've heard the quote above several times.  I felt like there are some misconceptions on what modern web development is among even seasoned embedded developers. My hope is that this post will fill in some of the gaps.

During my career I've switched between application development/web development and the embedded world. At the best of times, I've had the pleasure to combine them both. This post is targeted at developers more experienced in embedded, real-time, dedicated, or hardware-interacting systems. In this post I've tried to give an overview of what modern web development/enterprise computing is about. Those of you who are experienced enterprise computing, back-end, front-end, full-stack, web-gurus will not learn anything new here. Instead, stay tuned for a post on embedded development.

# Architecture

As a embedded developer you might wonder why there is a need for all these fancy frameworks when the problem is fairly simple: the browser sends some text over a socket, the server responds with some other text, and the browser converts this text to something the user can view. As a embedded developer, given that you have a good socket library, you can easily write a simple web server/web application in a handful lines of code.
So yes, this simplified problem can be solved using a fairly simple solution. However, modern web frameworks and web architectures are dealing with the following additional challenges:
- Complexity: a modern web application could have hundreds of separate features each offering some functionality and a different user interface view to the user. A goal of modern web framework is to simplify the programming model to deal with this complexity.
- User experience: a modern web application has to offer a great user experience on a wide variety of devices.
- Concurrency: a modern web application will need to serve possibly millions of concurrent users.

The modern web architectures of today are driven by providing solutions to these challenges.
Before I get into what the trends are today, I'll just briefly mention some of the historical web architectures.

## CGI ##
The programmable web started out with so called CGI scripts or CGI applications. These were typically written in C++ or Perl. This architecture was mainly used in the 1990s but are not used anymore for various reasons. It is hard to get a lot of performance out of a web server using this approach. In addition, the programming model didn't lend itself well to more complex applications. Finally, the user experience was not the best since every interaction with the user meant a round-trip to the server for a new page to be loaded.

## Three-tier ##
The next architecture I want to mention is the three-tier architecture that was reigning supreme for a long time. Java Enterprise Edition (also called Java EE, J2EE, Jakarta EE) was the main early framework for three-tier applications. The configuration mostly used today is Java together with the Spring framework. An alternative is to use C# using ASP.NET from Microsoft. In this architecture, there are three tiers: a presentation tier, a business logic tier and a database tier. In the typical three-tier solution, a page in the presentation layer is created on the server side by taking a HTML template and interleaving the HTML code with code that customizes the HTML. So just as with CGI scripts the page is created on the server. To introduce better response in the user interface some of the logic was in later years moved to the browser as Javascript. The Javascript library jQuery was the dominant library solution for making the presentation layer more responsive for many years. The main difference from the CGI model is that the presentation layer does not include any important business logic. Instead this is handled by the business logic tier. In addition, all storage needs are handled by the database tier in the form of a relational database. There there are tons of people still developing web application using this architecture, both for maintenance and new projects. In the last few years, however, modern web development is turning away from this model.
In my opinion, there are three main changes to the three-tier architectures that defines modern web development:
 - Single-Page Application
 - Microservices and RESTful interfaces
 - NoSQL databases.

## Single Page Application (SPA) ##
Instead of having most of the view being created on the the server side (if no jQuery is used), a modern web application has all the user interface logic executed in the browser using Javascript. The communication with the server is still over HTTP but instead of sending HTML ready to be rendered, it is now data in JSON format that is being sent. So no more of clicking a button and waiting while a new page is computed and sent back from the server. Instead, clicking a button in a SPA will typically change the view instantaneously and then when data from server is available the view gets updated again. Frameworks/libraries like Angular or React are dominating the SPA front-end development.

## Microservices and RESTful interfaces ##
The second big change is that the back-end that used to be one big monolithic application is now broken up into smaller services called microservices. These smaller services typically all communicate with each other or the SPA application in the browser using JSON over HTTP. The way these services are organized and how they are using the HTTP protocol is referred to as REST or RESTful. So now there really isn't a three-tier architecture anymore. Instead there are a number of independent services collaborating. Another key difference is that the different parts of the complete application can now be written in different languages, be hosted on different servers and use different databases to store persistent information.

## NoSQL databases ##
As mentioned before, the older three-tier architecture typically used a relational database as the database tier. For a lot of applications today short response time (low latency), high availability, and the ability to scale to millions of users are crucial. Here, NoSQL database like Mongodb, Couchbase or DynamoDb to name a few are a better fit than a traditional relational database. NoSQL databases have downsides as well and if you have transactions that span over several operations, a relational databases is still your best choice.


# Programming languages #
As a embedded developer or developer of real-time systems you typically only have two languages to choose from: C or C++. The web-world is different and you have a wide variety of languages that can be used. All languages used today for web development both on the front-end (browser) or the back-end (server) heavily rely on dynamic memory allocation and garbage-collection. Big enterprise computing back-ends traditionally relied on two statically typed languages: Java and C#. Nowadays, there are a wide variety of languages in use: Python, Ruby, Javascript (node.js), Scala, and Elixir to name a few languages used for the back-end. As mentioned before, Javascript is the language used on the front-end to make the Single Page Applications. A trend in later years, however, is to write the SPA in another language and compile it (or transpile as some people call it) into Javascript.

# Communication protocols
At least for more complex embedded systems, quite a bit of effort usually goes into designing and handling various communication protocols. As a web developer things are easier. First of all you rarely need to design or implement a communication protocol. You only have to deal with the topmost layer of the OSI stack, the Application layer, when you design your RESTful APIs. The rest of the stack is always almost always the same: HTTP over TCP over IP. Note that there are exceptions to the request-response pattern of HTTP. Websockets, for example, enable bi-directional communication from the Javascript running in the browser to the server. Another exception is when microservices communicate using remote procedure calls using some binary protocol like Google Protocol buffers instead of using REST and HTTP.

# Tools
An area where the web development really shines compared to what you are used to as an embedded developer is the tools area. There are a lot of really good IDEs available. My favorite are IntelliJ for Java or Scala and the sister product PyCharm for Python but there are freely available IDEs for any language or environment. There are even IDEs that run in your browser (like AWS Cloud9). Tools for unit testing, code coverage or static code analysis are also more readily available for languages used in web development compared to tools for embedded development. There are also much better systems for managing builds, dependency management and packaging releases. Gradle for Java, sbt for Scala, bundler for Ruby, pip/PyPI for Python are some examples.

# What I didn't cover
- Differences in running a project compared to embedded world
- Differences in unit testing
- Differences in testing in general
- Differences in productivity and differences in how long it takes a developer to becomes skilled
- The cloud, container and serverless trends.
