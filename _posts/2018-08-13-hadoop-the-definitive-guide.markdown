---
layout: post
title: "Review: Hadoop: the Definitive Guide, 4th ed"
date: 2018-08-13
category: bookreview
tags: hadoop programming bigdata
---

Since it is an O’Reilly [book](https://www.amazon.com/Hadoop-Definitive-Storage-Analysis-Internet/dp/1491901632), it’s a pretty solid read. However, it is not one of the publisher’s best.
It’s packed with information but suffers from trying to be broad and deep at the same time.
White’s definitive guide to Hadoop covers everything from the basic Hadoop framework (Hadoop 2 only) with MapReduce, HFDS and YARN to application frameworks like Hive, Pig, Spark, Oozie, Crunch.

The coverage is broad but I would have preferred a little more information on strategies to use.
Prior to reading this book I had had some exposure to Hadoop 1 and MapReduce and I had cursory knowledge of Hive and Spark. So I was more interested in when to pick up Hive vs SparkSQL and more information on if you do run Hive should it be run on Spark, Tez or MapReduce as underlying engine.

Instead the book gives you explanation of all the tools in the zoo but no expert guidance in when to use which.

On the deep end, there is plenty of information on how to create MapReduce job. In my opinion, those details you can and should pick up from the online documentation instead. But I’m grateful that another example than wordcount is used to explain MapReduce.

The book concludes with two applications: patient data consolidation and genomic data/sequence data mining. Interesting topics but I would have like to see a lot more detail here. At least details on the architecture level.

To summarize, I’m on the fence to recommending this book. It has a lot of knowledge but if you are a seasoned pro you will be asking for more and dive directly to online documentation and if you are new to Hadoop world you will be bogged down with details and struggle to see the big picture.
