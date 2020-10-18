---
title: "Capacity Engineering"
excerpt: "But can you do it at scale?"
---


# What is Performance? What is Capacity?
Latency. Throughput. Efficiency (cost). Scalability.

> *"The highest throughput, with acceptable latency, in the smallest footprint"* -- Performance and Capacity Engineering @ Facebook

Things to consider:
- Architecture
- Code base
- Infrastructure
- Hardware

## Architecture
Most applications have several components. Those components perform different tasks and have different resource requirements. Some are data intensive, some of computational intensive, ect.

## Horizontally Integration
Dedicated to communication between other subsystems.


## Vertically Integration
The process of integrating subsystems according to their function. Quick, involves only the necessary vendors.

## Top-Down Design
An overview of system is formulated without going into details for any part. Each part of it then refined into more details.

## Bottom-Up Design
Individual parts of the system are specified in details. The parts are then linked to form larger components.


## CPU- Central Processing Unit
More versatile than GPU's, but not as fast. Individual CPU cores are faster and smarter than individual GPU cores, but not as a hold. Suited for a wide variety of workloads, especially with per-core performance.

Constructed from millions of transistors, the CPU can have multiple cores.  

## GPU - Graphical Processing Unit
GPUs can process data several order of magnitude faster than CPU due to parallelism. GPUs are better for repetitive and highly-parallel computing tasks. Excel in machine learning, simulations, and scientific computation.

A processor that is made up by many smaller and more specialized cores. By working together, the cores deliver massive performance because it is divided up and processed across many cores.  




## Analytics in Capacity engineering
[Brendan Greegg's Flame Graphs](http://www.brendangregg.com/flamegraphs.html)

> *"Without data, and with analysis of data, you're not working on performance. You're working on something else"* -- Bill Jia, Facebook


Machine learning needs a lot of computing. Sometimes your servers have fixed computation and memory.



[Bill Jia](https://www.linkedin.com/in/billjiafacebook/) (Facebook) seems to be the expert on this in the world.
