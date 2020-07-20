---
title: "Hadoop"
excerpt: "Facilitate using a network of many computers to solve problems"
---

## What is it?
The Apache Hadoop project is an open-source software for reliable, scalable, distributed computing. It allows for the distributed processing of large data sets across clusters of computers. It scales up from single servers to thousands with local computation and storage.

Hadoop splits files into large blocks and distributes them across nodes in a cluster. It transfers packaged code into nodes to process the data in parallel.

The Hadoop framework itself is mostly written in Java.

Fun Fact: "Hadoop" was the name of a yellow toy elephant owned by the son of one of the inventors.

## Why is it important?
- Ability to store and process huge amounts of any kind of data, fast.
- Makes big data fast through distributing computing.
- Fault Prevention through redirection of failing node jobs.
- No preprocessing required before storing the data
- Open-source, free

Uses MapReduce Programming. MapReduce is a parallel processing software framework. Map step is a master node that takes inputs and splits it into smaller subproblems and then distributes it to worker nodes. Then, the master node takes all the answers of the subproblems and combines them to make the output.



## Data Warehouse
Data warehouses store vast amounts of structure data in highly regiment ways. It is pretty much synonymous with extract, transform, and load (ETL) processes. It requires rigid schemas (usually star or snowflake).

Data is refreshed periodically, usually at strange times when people aren't using it.

This analogy is that this data is like bottled water. The bottling has to be done and it has to be purified and it has to be packaged. Then it is ready for others to consume.

## Data Lake
A data lake is not a data warehouse. Data lake is a resting place for raw data in its original form until it is needed. In other words, data lies dormant unless someone needs it.

To access data lakes, users usually have to specify the data types they need and sources, how much they need, when they need it, and any analytics.

The schema is not predefined, it is ad hoc. The data can be structured, semistructured, or unstructured. It is natural and you would only drink it if you were in serious need of thirst. If you need 50 gallons, you don't need to buy the cases of bottled water and empty them one by one, instead, you get it all; ready to go.

RDBMs aren't designed to handle gigabytes or petabtyes of unstructured data. It is hard to have so many photos, videos, and emails in a traditional SQL severs and running reports or SQL statements.



## Other Software That Are Important
- Hive
- Spark 
