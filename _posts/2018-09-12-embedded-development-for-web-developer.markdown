---
layout: post
title: "Embedded development for Web developers"
date: 2018-09-12
category: blog
tags: embedded programming
---

> "Embbedded, that's assembler right? I remember we had to do that in college."

This is the second post on the differences between "embedded development" and "web development". The background behind these posts are two-fold:
 - I've had this idea of writing a more comprehensive book on SW architectures spanning the whole range from embedded systems to user interfaces and cloud. This post can be seen as a warm-up to that activity.
 - I have a background in both the enterprise computing/web application world as well as the embedded domain and from discussions with web developers I know that there is an interest in learning more about embedded systems. So in this post I set out to answer questions like the one above about assembler.
The target audience is anyone that sees themselves as a web developer and who would like to learn a bit more about embedded development. Embedded developers will not learn anything here.

# Definitions and Introduction

When I talk about embedded development in this post I mean a software system that is part of larger system and that that larger system has a dedicated purpose. A better term for embedded system is actually dedicated system. So any software running on a commodity server or laptop is not an embedded system since although the application might be dedicated the OS and the hardware is not. A lot of embedded systems have real-time requirements meaning some tasks must complete by a certain time or they do not meet the requirements. Realtime aspects are common in embedded systems but not necessary to call it an embedded system. Note that for a bigger embedded system it is not uncommon for some pieces would have hard real time constraint but for other parts of the system being late sometimes is acceptable. Another definition is firmware as a term for embedded software. I prefer to call it software and will use that term in this post.

# Architecture

I'm not going to cover hardware architectures in this post although for an embedded system, hardware and software will usually influence each other. Let's for a minute assume that our system, from hardware perspective, only has one microcontroller. These architectures can be used in a multi-node system as well but that is beyond this post. In  my experience the software architecture of a modern embedded system would fall into one of these three categories

## Architecture 1: Background-foreground

The first one is the classical background-foreground or main loop-interrupt handler architecture. Here, no operating system or third party task scheduler is used.
Instead the logic is split between interrupt handlers and one single never/ending loop that handles messages.
```C
int main(void) {
    for (;;) {
    //Code go here
  }
}
```
An interrupt handler gets called when external stimulus, like UART receiving a byte or some sensor triggering interrupt pin. The interrupt handler does minimal processing and instead puts a packet on the one message queue. The main loop is running whenever the interrupt handler is not running. It's job is simply to handle what is next on the queue or alternatively wait until something is there. Example of something you can build with this architecture is the firmware in a pacemaker. Very simplified, a pacemaker has some leads connected to the pacemaker and the actual lead go through the veins into the heart. The pacemaker will have a small microcontroller and specialized hardware that can sense the heart beating. In this simplified example, the most important interrupt is when dedicated sensing hardware senses that the heart beats. So there will be a interrupt controller for that event. A human heart beats about 1-2 times per second which leaves the background loop plenty of time to get the event and figure on what to do next. Should it help pace the heart and if so with what voltage. Back to the general, background-foreground architecture, it is safe to say that this architecture is typically only used in smaller systems. It starts to get a little complicated when the single microcontroller has more than one task with different timing characteristics. An example could be a system that is on one hand reading temperature data and controlling temperature by running a control loop and on the other hand is communicating with an external system. Your one main loop now have to handle two things with widely different timing concurrently. This leads us into the next architecture

## Multiple threads using and RTOS

This type of architecture really doesn't differ much from how some people in the web domain uses threads. The theory is the same but in an embedded system, the latency requirements can be tougher than in an enterprise computing application.
Next alternative that is widely used is using multiple threads by using a real time operating system (RTOS). Example of RTOS used today are FreeRTOS, VxWorks, ThreadX or QNX. Having threads simplifies handling several concurrent tasks compared to background foreground but introduces synchronization between threads. In this architecture you will still use interrupt handlers but instead of one main loop serving the messages that the interrupt handlers are putting on the queue there are multiple threads and multiple queues used. Synchronizing between threads is typically handled with mutexes protecting the shared resource. This type of architecture really doesn't differ much from how some people in the web domain uses threads. The theory is the same but in an embedded system, the latency requirements can be tougher than in an enterprise computing application. But all in all the problems I've seen here are the same as I've seen in enterprise computing. Thread synchronization is hard if you rely on low-level primitives.

## Actor model should always be used!

So the final architecture, the Actor model,  is the one that you should always use UNLESS you after careful consideration find out that can't. The actor model for an embedded system as it is normally used is not fully compliant with [Carl Hewitt's](https://en.wikipedia.org/wiki/Carl_Hewitt) original ideas but I still call it actor model. If you have used Akka for Java/Scala or Erlang, this is the same model but now adapted for an embedded system. So all the logic is now contained in a number of collaborating actors. The actors keep their own private state. The ONLY way you can share data is through messages. So there are no mutexes or other synchronization primitives needed. In a higher level language and server back-ends the actor model (Akka or Erlang) can create and destroy actors and create more (scale) if needed. For the embedded architecture in this post, creation of actors is performed once on start up since we want all memory to be allocated once and for all. So the actor model for embedded systems means:
- message passing only way of communicating
- each actor acts asynchronously from the other actors. This could be done by having an underlying thread per actor but it is also possible to use a thread pool.
- any piece of state in the system is owned by an actor. This is truly encapsulation since only a message can be sent to actor, no other way of getting any information about state.


# Programming languages
Sure there are some rare cases where good ol' assembler is used but this is extremely rare. C or C++ reign supreme. Compiler front-ends (the part of the compiler producing machine code, we are not talking Javascript here...) are so good nowadays so it's tough to beat it with assembler. I make a distinction between C and C++ although you can certainly mix them in a project. Typically I see more C in a small systems using background-foreground architecture or small memory-print RTOS and more C++ on bigger systems using actors but all combinations are possible. Higher-level languages can certainly also be used in a embedded system but typically not directly controlling hardware or handling time-sensitive signals. I've seen embedded system with a small user interface where the user interface logic and higher level logic were implemented in Java or C# using Android or Windows embedded (Windows IoT) but the low-level control loops where running on a separate microcontroller implemented in C/C++.


# Communication protocols

If web developers have a plethora of languages to choose from to solve one communication problem sending text over a socket, the embedded developer have tons of communication problems to solve with only C and C++.
Low-level communication with sensors or actuators sitting on the same PCB as the microcontroller would use I2C or SPI.  Serial communication using a UART is also heavily to communicate with things on a different PCB.
Next level would be between microcontroller/microprocessors where again serial (UART), CAN bus or even Ethernet can be used.
A lot of embedded systems today or communicating user wireless protocols like Bluetoorh/BLE, Zigbee, wifi, 3G networks etc.
As software developer, it is likely that you will spend a lot more time on the communication solution than you would in a web project where everything is already handled for you. There is a trend in embedded systems to use TCP/IP as much as possible but there are plenty of specialized transport protocols out there. So a lot of embedded developers are designing or implementing communication protocols where protocol versioning, small protocol overhead, easily parsed message headers etc, good compression, built-in integrity checks are crucial.


# Tools

The final area that I wanted to cover are developer tools. This is an area where you as a web developer will feel like you have taking a time machine back at least 10 years. A lot of embedded developers are still using Eclipse with old plugins, Slickedit or some proprietary IDE coupled to the microcontroller vendor you picked. Furthermore, instead of a choice between Linux, Mac or Windows for your host machine, you only get to choose Windows since a lot of emulators and hardware debuggers are only supported on this platform. This is changing and in a few years landscape might seem different. The actual compiler (and linker) is another area which is more complicated for embedded system. Since you typically want to build for both your target system and for your host (to run unit test or some sort of integration test) you will need to handle several different compilers and build targets. And the build systems used to handle all these build targets is another sore point for embedded systems development. A lot of companies are still using make from the seventies. There are some advances like CMake but if you are used to something like gradle for Java you will be amazed how complicated building a larger embedded system can be.  

# What I didn't cover
Hardware skills needed for embedded development. Why it takes longer to become really good embedded developer. Maybe for another post.
