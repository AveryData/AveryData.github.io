---
title: "Graph Analytics"
excerpt: "Graph Theory"
---

Graph or network data.


Networks are everywhere! Think of all the examples. Each webpage is a node and there are links (edges) going all over. Facebook profiles are node with friends (edges). Twitter is who follow who. Cellphone network, who calls who. Amazon, who buys what!

Understanding graphs is best done via network diagrams with nodes and edges. Edges can be directed (via an arrow), and weight can be seen by thickness.

Next most common is Adjacency matrix that shows connections in a symmetrical table. You end up with a sparse table. To make it less spare you'd get an adjacency list.

Could also just use an edge list which is a big table between nodes.

ID's are assigned to nodes via *map* in Java and *dictionary* in Python, or SQLite.

How to store the long list? On your laptop using SQLite or Neo4j (graph database), on a server, with MYSQL or PostgreSQL, or a cluster of machines via Titan or Hadoop.


## What to do with graph data?

Are they any patterns we see?

Are there any outliers?

Are there any patterns we expected to see that we don't see?

Come up with recommendations? Would Bob like to be friends with Joe?  

Random graphs would have independent edges and equal chance of forming which makes no obvious patterns.


Diameter is how far nodes are away from each other. Effective diameter is 90% of the distance (gets rid of outliers).

## Centrality (Importance)
Who are the influential or celebrities or gatekeepers?

It ranks the nodes and shows the ones that are most relevant.

**Degree Centrality** is an easy way to rank! How many edges does the node have? The more important it is.

**Betweenness Centrality** is basically showing how much of a connector or gatekeeper someone is. Who is the bridge? You look at all the shortest paths that go through that point.

**Clustering Coefficient** is how close the nearest neighbors are. If your neighbors know each other, you have a high coefficient.  Highest value is 1 (all your friends know each other). Lowest value is 0 where none of your friends know each other. You can count triangles fast with eigenvalues using the *Super fast triangle counting technique*. We only need the first few eigenvalues.

**Page Rank** comes from Google to rank webpages. What website is the most interesting? It's interesting if it is connected with other nodes. You start by using a random walk and whatever one has the highest steady state probability (ssp). If you started on any random website and rolled a dice to go to any neighbors, where would you end up.

**Personalized Page Rank** is when the page rank isn't the same for every person. I might want more relative pages to me, then just in general. There is just one change, instead of a random node, we use a preferred node.
