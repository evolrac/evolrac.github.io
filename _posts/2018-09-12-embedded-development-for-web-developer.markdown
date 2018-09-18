---
layout: post
title: "Embedded development for Web developers"
date: 2018-09-12
category: blog
tags: embedded programming
---

> "Embbedded, that's all assembler right? I remember we had to do that in college."

This is the second post on the differences between "embedded development" and "web development". The background behind these posts are two-fold:
 - I've had this idea of writing a more comprehensive book on SW architectures spanning the whole range from embedded systems to user interfaces and cloud. This post can be seen as a warm-up to that activity.
 - I have a background in both the enterprise computing/web application world as well as the embedded world and from discussions with web developers I know that there is an interest in learning more about embedded systems. So in this post I set out to answer questions like the one above about assembler.

The target audience is anyone that sees themselves as a web developer and who would like to learn a bit more about embedded development. Embedded developers will not learn anything here.

# Definitions and Introduction

When I talk about embedded development in this post I mean a software system that is part of larger system and that that larger system has a dedicated purpose. A better term for embedded system is actually dedicated system. So any software running on a commodity server or laptop is not an embedded system since although the application might be dedicated the OS and the hardware is not. A lot of embedded systems have real-time requirements meaning some tasks must complete by a certain time or they do not meet the requirements. Realtime aspects are common in embedded systems but not necessary for it to be called an embedded system. Note that for a bigger embedded system it is not uncommon for some parts of the software to have hard real time constraints but for other parts of the system being late could be acceptable. Some people use the term firmware for embedded software. I prefer to call it software and will use that term in this post.

# Architecture

I'm not going to cover hardware architectures in this post although for an embedded system, hardware and software will usually heavily influence each other. In this post, consider that our system, from hardware perspective, only has one microcontroller. Embedded architectures can definitely also come in a multi-controller configurations but that is beyond the scope of this post. In  my experience the software architecture of a modern embedded system would fall into one of these three categories

## Architecture 1: Background-foreground

The first architecture is the classical background-foreground or main loop-interrupt handler architecture. Here, no operating system or third party task scheduler is used.
Instead the logic is split between interrupt handlers and one single never/ending main loop that handles messages.
```C
int main(void) {
    for (;;) {
    //Code go here
  }
}
```
An interrupt handler gets called when external stimulus, like UART receiving a byte or some sensor triggering interrupt pin. The interrupt handler does minimal processing and instead puts a packet on the one single message queue. The main loop is running whenever the interrupt handler is not running. The job of the main loop is simply to handle what is next on the queue or alternatively wait until something is there. Example of something you can build with this architecture is the firmware in a pacemaker. Very simplified, a pacemaker has some leads connected to the pacemaker in one end and to the heart in the other. The pacemaker has a small microcontroller and some specialized hardware that can sense the heart beating. In this simplified example, the interrupt gets triggered when the dedicated sensing hardware senses that the heart beats. So there will be a interrupt handler for that event. A human heart beats about 1-2 times per second which leaves the main loop plenty of time to get the event from the queue and compute what to do next. Should it help pace the heart and if so with what voltage. Back to the general, background-foreground architecture, it is safe to say that this architecture is typically only used in smaller systems. It starts to get a little complicated when the single microcontroller has more several tasks with different timing characteristics. An example could be a system that is on one hand reading temperature data and controlling temperature by running a control loop and on the other hand is communicating with an external system. The single main loop now have to handle two things with widely different timing concurrently without having any mechanism for handling concurrency. This leads us into the next architecture.

## Architecture 2: Multiple threads using and RTOS

The next architecture that is widely used is using multiple threads by using a real time operating system (RTOS). Example of RTOS used today are FreeRTOS, VxWorks, ThreadX or QNX. Having threads simplifies handling several concurrent tasks compared to background foreground but introduces synchronization between threads. In this architecture you will still use interrupt handlers but instead of one main loop serving the messages that the interrupt handlers are putting on the queue there are multiple threads and multiple queues used. Synchronizing between threads is typically handled with mutexes protecting the shared resource (shared state). This type of architecture really doesn't differ much from how some people in the web domain uses threads. The theory is the same but in an embedded system, the latency requirements can be tougher than in an enterprise computing application. But all in all the problems I've seen here are the same as I've seen in enterprise computing. Thread synchronization is hard if you rely on low-level primitives like mutexes. This brings us to the final architecture.

## Architecture 3: Actor model

So the final architecture, the Actor model,  is the one that you should always use UNLESS you after careful consideration find out that can't. If you have used Akka for Java/Scala or Erlang, this is the same model but now adapted for an embedded system. The actor model for an embedded system as it is normally used is not fully compliant with [Carl Hewitt's](https://en.wikipedia.org/wiki/Carl_Hewitt) original ideas but I still call it actor model. The idea is that all the logic is now contained in a number of collaborating actors. The actors keep their own private state. The ONLY way you can share data is through messages. So there are no mutexes or other synchronization primitives needed. In a higher level language in a server back-end the actor model (Akka or Erlang) can create and destroy actors and create more (scale) if needed. For the embedded architecture in this post, creation of actors is performed once on start up since we want all memory to be allocated once and for all. So the actor model for embedded systems means:
- message passing is the only way to communicate with an actor
- each actor acts asynchronously from the other actors. This could be done by having an underlying thread per actor but it is also possible to use a shared thread pool.
- any piece of state in the system is owned by an actor. This is truly encapsulation since only a message can be sent to actor, no other way of getting any information about state.

Note that the actor model uses a RTOS for the underlying threads but the developer of the embedded application doesn't interact with the threads directly.
Another benefit of the the actor model is that it can easily be extended to more complex embedded systems with several interacting microcontrollers.

I see a parallel with the use of actors in embedded systems with the trends in enterprise systems. Just as the best way to implement a complex enterprise computing system is to use microservices that keep their state private and only interact using messages, the best way to implement a complex embedded system is using actors that keep their state private and only interact using messages.


# Programming languages
Sure there are some rare cases where good ol' assembler is used but this is extremely rare nowadays. C or C++ reign supreme. Compiler front-ends (the part of the compiler producing machine code, we are not talking Javascript here...) are so good nowadays so it's tough to beat it by hand-crafting assembler code. Note that I make a distinction between C and C++ although you can certainly mix them in a project. Typically I see more C in a small systems using background-foreground architecture or small memory-print RTOS and more C++ on bigger systems using actors but all combinations are possible. Higher-level languages can sometimes be used in a embedded system but typically not directly controlling hardware or handling time-sensitive signals. I've seen embedded system with a small user interface where the user interface logic and higher level logic were implemented in Java or C# using Android or Windows embedded (Windows IoT) but where the low-level control loops where running on a separate microcontroller implemented in C/C++.


# Communication protocols

If web developers have a plethora of languages to choose from to solve one communication problem sending text over a socket, the embedded developer have tons of communication problems to solve with only C and C++.

As an embedded developer you might be tasked with writing a driver for communicating sensor or actuator sitting on the same PCB as the microcontroller using use I2C or SPI. Or you might be tasked with implementing a message protocol using serial communication over RS-232 to communicate with another microcontroller. Or you might use CAN bus or Ethernet for inter-microcontroller communication.
As an embedded software developer, it is likely that you will spend a lot more of your time on the communication solution than you would  as a web developer. There is a trend in embedded systems to use TCP/IP as much as possible but there are plenty of other specialized transport protocols out there as well. In short, more of the OSI stack is implemented in an embedded project. So things like protocol versioning, small protocol overhead, easily parsed message headers etc, good compression, when and how to put in integrity checks/checmsums in your protocol are challenges that you might face as an embedded developer.


# Tools

The final area that I wanted to cover is developer tools. This is an area where you as a web developer will feel like you have taking a time machine back at least 10 years. A lot of embedded developers are still using Eclipse with old plugins, an old version of Visual Studio, Slickedit, or some proprietary IDE from the microcontroller vendor. Furthermore, instead of a choice between Linux, Mac or Windows for your host machine, you are often stuck with Windows since a lot of emulators and hardware debuggers are only supported on this platform. This is changing and in a few years landscape might look very different. The actual compiler (and linker) is another area that is more complicated in embedded systems development. Since you typically want to build for both your target system (microcontroller) and for your Windows host (to run unit test or some sort of integration test) you will need to handle several different compilers and build targets. And the build system used to handle all these build targets and dependencies is another sore point for embedded systems development. A lot of companies are still using make from the 1970s. There are some advances like CMake but if you are used to something like gradle for Java you will be amazed how complicated building a larger embedded system can be.  

# What I didn't cover
 - Hardware skills needed for embedded development
 - Why it takes longer to become really good embedded developer.
