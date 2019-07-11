---
layout: post
title: "Storm @ Twitter"
comments: True
categories: "big-data"
---

Storm is a real-time fault-tolerant and distributed stream data processing system. It is a current in use system at Twitter for scalable real-time data computation.

<!-- More -->

* Problem and motivation
  * Lack of "Real-time Hadoop"
    * The motivation of Storm is building a system to deal with real-time data processing at massive scale.

* Design Decision
  * Scalable
    * Storm scales to massive numbers of messages per second. To scale a topology, all you have to do is add machines and increase the parallelism settings of the topology. As an example of Storm's scale, one of Storm's initial applications processed 1,000,000 messages per second on a 10 node cluster, including hundreds of database calls per second as part of the topology. Storm's usage of Zookeeper for cluster coordination makes it scale to much larger cluster sizes.

  * Fault tolerant
    * If there are faults during execution of your computation, Storm will reassign tasks as necessary. Storm makes sure that a computation can run forever (or until you kill the computation).
    Programming language agnostic: Robust and scalable realtime processing shouldn't be limited to a single platform. Storm topologies and processing components can be defined in any language, making Storm accessible to nearly anyone.

  * Extensible
    * Storm can be used for processing messages and updating databases (stream processing), doing a continuous query on data streams and streaming the results into clients (continuous computation), parallelizing an intense query like a search query on the fly (distributed RPC), and more. Storm's small set of primitives satisfy a stunning number of use cases.

  * Robust
    * A realtime system must have strong guarantees about data being successfully processed. A system that drops data has a very limited set of use cases. Storm guarantees that every message will be processed, and this is in direct contrast with other systems like S4.
    Extremely robust: Unlike systems like Hadoop, which are notorious for being difficult to manage, Storm clusters just work. It is an explicit goal of the Storm project to make the user experience of managing Storm clusters as painless as possible.

* Data Model
  * Streams of tuples flow through topology
  * Spout
    * Source of data stream
  * Connection
    * Shuffle grouping
    * Fields grouping
    * All grouping
    * Global grouping
    * Local grouping
  * Bolt
    * Executor

* System Architecture
  * Nimbus
  * ZooKeeper
  * Worker Node
    * Supervisor Process
    * Worker Processes
