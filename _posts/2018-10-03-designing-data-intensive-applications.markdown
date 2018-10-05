---
layout: post
title: "Review: Designing Data-Intensive Applications, 1st ed"
date: 2018-10-03
category: bookreview
tags: algorithms programming databases
---

Just finished Martin Kleppmann’s [Designing Data-Intensive Applications](https://www.amazon.com/Designing-Data-Intensive-Applications-Reliable-Maintainable/dp/1449373321/).

It’s published by O’Reilly but not your typical O’Reilly book heavy with code examples and made to read wit the IDE or REPL handy to try things out as you read. Instead this is more of a classic college text-book and I wouldn’t be surprised if it is or will be used as such.

This book takes a holistic view of writing, reading, storing, and working with data in modern highly available, high throughput, scalable and resilient server applications.

Having worked as software engineer and manager for close to 20 years and much of that involving some kind of databases, this is the first time I have read a book that explained the whole picture.

This is not a how-to book for solutions architects or database designer. And it is certainly not a book if you want to learn a particular tool or database. Particular tools, databases, systems, and frameworks are mentioned but there is no information on how to use a particular system or even a comparison of different tools. Instead this is a book about challenges, or maybe I should say problems, everyone working in this space will face and the solutions to these problems.

The problems and solutions are presented with just the amount of theory. There are no mathematical proofs in the book, for that you have to go to the research papers, but there is enough meat for all save PhD students and researchers.

To be honest, I didn’t learn that many new things from this book, but it made me (finally) understand how a Kafka stream, a Redshift datawarehouse and a shopping cart stored in MongoDB all relate and what choices have been made in these different systems with respect to transactions, locks, availability, read/write speed, loose coupling etc. Kleppmann does a wonderful job of showing how it all fits together and what trade-offs you make if you choose a particular solutions vs another.

I can highly recommend this book for almost everyone with an interest in distributed systems and database design:
- If you are just getting started in this space, I wouldn’t recommend this as the first book. Read [Seven Databases in Seven Weeks](https://pragprog.com/book/pwrdata/seven-databases-in-seven-weeks-second-edition) first. This is a very hands-on book but very light on theory.  After you have some experience, you are ready for Kleppmann’s book.
- If you are in the middle of your career and feel that you know you distributed systems but you have this nagging feeling that there is some deeper truth out there, this is the book for you.
- And finally, if you are the a master of distributed systems and know everything there is to know about databases, read this just for fun. Kleppmann’s style make it an easy read and there are tons of historical references if you want to dig deeper.
