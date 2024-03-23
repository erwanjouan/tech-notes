---
title: "Patterns of entreprise application"
date: 2021-03-24T14:06:36+01:00
draft: false
---

### Introduction

#### Thinking about Performance

- Think about performance, many architectural decisions are about performance.
  - Get the system up and running
  - Instrument it
  - Use a disciplined optimization process based on measurement

- Guidelines:
  - Minimize remote calls
  - Don't sacrify readability for performance 
  - Any configuration change can invalidate performance optimization

- In general ***performance*** is either ***throughput*** of ***response time***. Use appropriate terminology for performance :
  - ***Response time***: amount of time it takes for the system to process a request from the outside.
  - ***Responsiveness***: how quickly the system acknowledges a request as opposed to processing it.
  - ***Latency***: the minimum  time required to get any form of response even if the work to be done is non existent. This is why you should avoid remote calls
  - ***Throughput***: how much you can do in a given amount of time. Typically transactions per seconds.

- ***Load***: is how much stress a system is under, which might be measured how many users are currently connected to it. Load adds a context for other measurements (e.g : response time is 0.5s with 1 user, 2s with 5 users)
- ***Load sensitivity***: is how measuremnt varies with load.
- ***Efficiency***: is performance divided by resources.
- ***Capacity*** of a system is an indication of maximum effective throughput or load. This might be an absolute maximum or a point at which the performance dips below an acceptable threshold.
- ***Scalability***: is a measure of how adding resources (usually hardware) affects performance. A scalable system is one that allows you to add hardware and get a commensurate performance improvement, such as doubling how many servers you have to double your throughput:
    - ***Vertical scalability*** or scaling up means adding power to a single server, such as memory.
    - ***Horizontal scalability*** or scaling out means adding more servers.

When building entreprise systems, it often makes sense to build for hardware scalability rather than capacity. 
Scalability is easier to do, often designers do complicated things that improve the capacity on a particular HW. 
Adding more servers of often cheaper than adding more programmers, providing that a system is scalable.

#### Patterns

Christophe Alexander, an inspiration for many patterns enthusiasts:
> Each pattern describes a problem which occurs over and over again in our environment and then describes the code of the solution to that problem, in such a way that you can use this solution a million times over without ever doing it the same way twice.

You can never just apply a solution blindly, you just need to read enough to have a sense of what the patterns are, what problems they solve and how they solve them.

Patterns help to communicate more effectively.

#### Structure of the Patterns

- Properties
  - Name of the pattern. This element is crucial because Pattern should ease communication
  - The intent
  - The sketch (visual representation or UML)
  - Motivation
  - How it works
  - When to use it
  - Further reading

A collection of patterns is by no means a comprehensive guide to entreprise application development.

### The narratives

Layering is one of the most common techniques that software designers use to break apart a complicated software system. Ex: network protocols.
