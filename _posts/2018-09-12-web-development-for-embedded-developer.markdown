---
layout: post
title: "Web development for Embedded developers"
date: 2018-09-12
category: blog
tags: web programming
---

> "Any one can do web development. Embedded development is much harder"

Working a lot with embedded developers, I felt like there are some misconceptions on what modern web development is among web developers. My hope is that this post will fill in some of the gaps.

I've spent my career switching between application development/web development and the embedded world. At the best of times I've had the pleasure to combine them.  This post is targeted at developer more experienced in embedded, real-time, dedicated, hardware-interacting systems. Those of you who are experienced enterprise computing, back-end, front-end, full-stack, web-gurus this will not learn anything new here. Stay tuned for a post on what embedded development is all about.

# Architecture

As a embedded developer you might think wonder why there is need for all these fancy framework when the problem really is same fairly simple thing: the browser sends some text over a socket and server response with some other text whereby the browser converts this text to something user can view in the browser window. And as a embedded developer,  given you have a system with socket library you can write a simple combined web server/web application in a handful lines of code.
So the core problem can be solved with a fairly simple solution. However, modern web frameworks and web architectures are trying to solve the problem with the additional caveats:
- Complexity: a modern web application could have hundreds of separate views and so a goal is to simplifying the programming model to deal with complexity
- User experience: a modern web application has offer a great user experience.  
- Concurrency: a modern web application will need to serve possibly millions of concurrent users.

The modern web architectures of today are driven by providing solution to these problems.
The programmable web started out with so called CGI scripts or CGI applications. These were typically written in C++ or Perl. This was mainly used in the 1990s but are not used anymore for various reasons but it is hard to get a lot of performance out of a web server using this approach. And the user experience was not the best since all interaction with the user meant a round-trip to the server.
Fast forward and the three-tier architecture entered the scene.
They were reigning supreme for a long time. This type of architecture is typically implemented in Java using Java Enterprise Edition (Java EE, J2EE, Jakarta EE etc). The framework mostly used today for this type of architecture today is Spring from Pivotal. Using Microsoft and the .NET (ASP.NET and friends) is an alternative. In this architecture there are three tiers: a presentation tier, a business logic tier and a database tier.  In the typical three-tier solution the presentation layer is using being created on the server side by taking a HTML template and interleaving the HTML code with code that customizes the HTML. To introduce better response in the user interface some of the logic is exectued in the browser using javascript. The javascript library jQuery was the dominant force for many years. There there are tons of people still developing web application using this architecture. Not just maintenance but also for new projects. However, modern web development is turning away from this model.
So modern web development introduced three big changes:
 - Single-page applications
 - Microservices and RESTful webservices
 - NoSQL databases.

## Single Page Application ##
Instead of having most of the user interface logic on the server side, a modern web application has all logic executed in the browser. The communication with the server is still over HTTP but instead of sending HTML is now data typically in JSON format being sent. So there is no clicking a button leads request to server and a new page is loaded. Frameworks /libraries like Angular or React is dominating.

## Microservices and RESTful interface ##
Second big change is that the back-end that used to be one big monolithic application is not broken up into smaller services that (typically) all communicate with each other or the SPA application in the browser using HTTP using the so called REST paradigm. So there really isn't three layers anymore.

## NoSQL databases ##
The older three-tier architecture typically used a relational database as the database tier. For a lot of applications today response time and availability is crucial. Here NoSQL database like Mongodb, Couchbase or DynamoDb are a better choice. NoSQL has some downsides like not supporting transactions like a relational databases.


# Programming languages #
As a embedded developer or developer of real-time systems you typically only have two languages to choose from: C or C++. So the web-world is different. All languages heavily rely on dynamic memory allocation that will later be automatically cleaned up (garbage-collected in Java speak). Big enterprise computing back-ends rely on two statically typed languages: Java and C#. A lot of smaller use dynamically typed languages like Python, Ruby or javascript for the backend (node.js). AS mentioned before javascript is the language used to make the Single Page Applications. A trend in later years although it is sitll much less used than javascript is to write the SPA in another language and compile it (or transpile as some people call it) into javascript

# Communication protocols
At least for more complex embedded systems, quite a bit of effort usually goes into communication protocols. As a web developer things are easier. First of all you rarely need to design or implement a communication protocol except for designing the actual messages at the highest level of the OSI stack. So in a web architecture communication almost exclusively means sending text to each other over request response protocol (HTTP) over TCP/IP. There are esxceptions like using websockets which enables bi-directional communication from the javascript running in the browser to the server. Another exception is when microservices communicate using some binary protocol like Protocol buffers instead of using text over HTTP.

# Tools
An area where the web development really shines compared to what you are used to as an embedded developer is the tools area. There are a lot of really good IDEs. My favorite is IntelliJ (and the sister product PyCharm for Python) for Java and Scala development but there are freely available IDEs for any language or environment. There are even IDEs that run in your browser (like AWS Cloud9). In opinion,  tools for unit testing, code coverage or static code analysis are also more readily available for languages used in web development compared to tools for embedded development that are usually tied to a specific platform and comes with a fee. There are also much better build systems/dependency management systems  (gradle for Java, sbt for Scala, gem for Ruby, pip/PyPI for Python) for web development compared to what you are used to for embedded systems. 

# What I didn't cover
Difference in running a project, differences in unit testing, differences in testing in general, differences in productivity, differences in developer maturity.
