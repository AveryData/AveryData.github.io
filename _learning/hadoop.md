distributed---
title: "Hadoop"
excerpt: "Facilitate using a network of many computers to solve problems"
---

## What is it?
The Apache Hadoop project is an open-source, free, low-cost solution for reliable, scalable, distributed computing. It allows for the distributed processing of large data sets across clusters of computers. It scales up from single servers to thousands with local computation and storage.

Hadoop splits files into large blocks and distributes them across nodes in a cluster. It transfers packaged code into nodes to process the data in parallel.

The Hadoop framework itself is mostly written in Java.

It also is fault tolerant (HDFS) and can recover machine/disk failure.

It should be linearly scaled, meaning if you have two machines, jobs run twice as fast (ideally).

Fun Fact: "Hadoop" was the name of a yellow toy elephant owned by the son of one of the inventors.

You can get Hadoop access via Microsoft Azure or AWS.

## Why is it important?
- Ability to store and process huge amounts of any kind of data, fast.
- Makes big data fast through distributing computing.
- Fault Prevention through redirection of failing node jobs.
- No preprocessing required before storing the data
- Open-source, free


### MapReduce
Uses MapReduce Programming. MapReduce is a parallel processing software framework. Map step is a master node that takes inputs and splits it into smaller subproblems and then distributes it to worker nodes. Then, the master node takes all the answers of the subproblems and combines them to make the output.

MapReduce divides data into small pieces and then combines the parts to give a result.

The Map Phase divides the data and computation into smaller pieces. The master computer decides how small to divide the pieces. Each worker gets a name, a mapper. It does any assignment the master has assigned to it. Mappers work individually and independently.

The Shuffle Phase takes the results from the workers and sends them together to "reducers".

The Reduce Phase combines everything from the smaller pieces.



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
- Hive = queries data stored from Hadoop, very similar to SQL
- Spark = better for machine learning tasks due to distributed memory
- Pig = easier way to make MapReduce programs. Automatically makes Hadoop.
- HBase

### Pig
A simpler way to write MapReduce platform. Instead of writing low-level map reduce functions, use pig. It is easier to understand and maintain. It automatically is converted to MapReduce programs. It is written in *pig latin*. The code becomes *data flow*. You don't have to write that many lines and it is easy to learn. It also makes it easy to do a sample run on a chunk of a large data set, debugging.

You trade off some speed for easy of use and code. The trade off is usually worth it.

Pig is proceduarl.

Runs on your computer, doesn't have to be installed on a Hadoop cluster.

You can write as scripts, and the second is through a command line (grunt).

### Hive (HiveQL)
Use SQL with large data sets stored on Hadoop. Runs on client side and doesn't need need to be installed on Hadoop cluster.

Hive is declarative.



### Spark
Some people called it Hadoop 2.0, but it isn't from Hadoop. It is a fast, MapReduce engine with in-memory data storage to speed up computation. Up to 100x faster than Hadoop Database. It is better for iterative algorithms (lots of machine learning and graph learnings benefits from this).

*Spark is way faster for iterative data. Hadoop is faster for 1 interaction.*

Compatible with Hive data, HiveQL, ect.

Spark supports Java, Scala, R, and Python.

Hadoop reads and writes to disk each scenario. In Hadoop, it iterates over the entire data set multiple times. If we use Spark, the data that needs to be iterated multiple times is kept in-memory. It uses RDD (resilient distributed datasets) to distribute the cached memory.

An example where Spark could come in handy would be Log Mining where load error messages from a lot are put into memory, then interactively search for various patterns.

It by default doesn't load anything into memory as it sees it as a scare resource; you decide what goes into memory.

#### Spark SQL
Hive is great, but Hadoop's disk-based engine can make even the smallest queries take minutes.

Spark is very similar to Hive but slightly different.

Spark Streaming is pretty good. It makes small batches and keeps them in memory. It can do like 4GB/s with 100 nodes.

Machine Learning Library or Mllib is also popular on Spark.

You want all the data to work with in memory, but Spark can still deal with running out of data.

### HBase
HBase is part of the Hadoop data system.

Built on top of HDFS.

It supports real-time read/write random access.

It is a NoSQL data base and is not relationship based.

It can support billions of rows, millions of columns.

It is written in Java but supports a lot of languages.

It is based off of a row key.

Columns are grouped into column families to help with organization, understanding, and storage optimization.

Cells are timestamped. The most recent values are read first.

Role keys must be unique.

It gets scaled up with storage regions.

It is sparse, distributed, persistent, multidimensional map.

It is easier to run with the interactive shell.

They're different from Relationship databases when you don't know the structure or schema before hand. It can handle sparse data really well. HBase keeps multiple versions of the data. It is generally cheaper to deploy HBase at big scales.
