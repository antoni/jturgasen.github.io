---
layout: post
title: "Tech Takeaway 039: Apache Kafka and the Next 700 Stream Processing Systems by Jay Kreps"
description: ""
category: 
tags: ["tech takeaway"]
---
{% include JB/setup %}

[Apache Kafka and the Next 700 Stream Processing Systems by Jay Kreps of Confluent at Strange Loop 2015](https://www.youtube.com/watch?v=9RMOc0SwRro)
 
Takeaways:

* He feels there are 3 paradigms for programming...
    * Request / response (one input, one output, and generally sync (vs async))
    * Batch (all inputs, send back all outputs)
    * Stream processing (some inputs, some outputs)
* Batch type computation is great for data locality since you can customize the workflow (eg: pull data local before processing it)
* Batch also allows for pre-processing
* He likes stream processing because it can be anywhere between request / response and batch (eg: 2 inputs, 3 outputs, X inputs, Y outputs, 10 inputs, 1 output, etc etc)
* Stream processing gives you total control of the tradeoff between latency and efficiency
* Stream processing is not...
    * Transient
    * Approximate
    * Lossy
    * It's the opposite of those things.  Those things are associated with stream processing due to the design of older, shitty, systems
* Stream processing is usually async / decoupled
* Gives an example using streams from a retailer (sales, shipments, fraud, price adjustments, etc)
* Talks about how stream processing fits in the processing time gap between batch (hours/days) and request/response (milliseconds)
* Hard problems a streaming system has to address:
    * Partitioning and scalability
    * Semantics and fault tolerance
    * Unifying tables and streams
    * Time (consistent view of time for all nodes)
    * Re-processing (failed/missed messages)
* **Producers** create the streams, **consumers** consume the streams
* Kafka has a **transaction log** or commit log - just like a DB or file system
* A stream of data can be considered "a sequence of records"
* Your app needs to maintain the state of where it's reading in the commit log!  Each app/client/consumer can be reading different points in the log
* Kafka has **log compaction** which will compress down the change/commit log so it requires less storage space
* Consumers can be put into **groups** .  Each consumer will process a subset of the streams
* All streams are multi-reader (aka topics are multi-subscriber)
* Talks about how change logs can be used to move forward and back in time, used for DR and recovery, etc
* Talks about re-playing logs to catch up with missed processing
* He talks about how consumers can change their **offset** to change "time" and re-process old streams
* Consumers can be apps that archive and process data (think Hadoop + HDFS)

A good overview of both the philosphy of and the tech behind modern streaming platforms.