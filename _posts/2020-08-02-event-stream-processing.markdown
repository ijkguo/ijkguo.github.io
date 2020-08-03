---
layout: post
title:  "Event Stream Processing"
date:   2020-08-02 15:57:30 -0700
categories: streaming
---

Event stream processing systems typically derives information from immutable data source with incremental update business logic. Backfill or reprocess is unavoidable because software evolves over time and different use cases diverge on latency and completeness requirements. Balance of batch vs streaming becomes critical for sustainable software.

## Lambda Architecture: Batch + Streaming

Originally proposed in [1], lambda architecture emphasized the importance of immutable data source and the key challenge of reprocessing:
* Immutable data source is what makes complex asynchronous transformations tractable. Transformations are composed of materialized stages, where each individual record  can be processed deterministically, repeatedly, and in parallel.
* Batch layer for reprocessing data is what limits the sacrifice of consistency to only the new data approximated by a real time layer. Limited by CAP theorem, lambda architecture chooses partitioning and availability over consistency.

Lambda architecture is composed of:
* Append-only, immutable data source with natural time-based ordering of data. Typical examples are HDFS, Hive, Kafka.
* Batch layer that produces the processing ground truth. Typical examples are Hadoop.
* Real time layer that makes up the gap of batch view and any new data. Typical examples are Spark Streaming, Kafka Streams.
* Serving layer that can combine batch view and real time view. Typical examples are Cassandra, elasticsearch.

As critized in [2], lambda architecture has the operational complexity of maintaining 2 separate code base (batch + streaming).

## Kappa Architecture: Run Backfill By Streaming It All

[2] proposed an alternative, "Kappa" architecture, to use a single streaming processing framework to backfill / reprocess data very fast. Approach is simple:
* Set up new data stream, output and persistent state tables.
* Start a new streaming job that reads from the new data stream and writes to the new output table, using the new persistent state storage.
* Replace the old output table with new output.

As pointed out in [3], "Kappa" architecture is tricky to implement in production when you try to stream all data and it is difficult to:
* Get it correct. The original event ordering from the old log source is different to replicate when reading from the complete archive stored in data warehouse.
* Get it under control. Dumping everything from data warehouse will cause lag of streaming pipeline, overflow downstream consumers, and the additional resources from shared clusters may not be available.

The optimization in [3] is to implement a new stream source that performs data warehouse query within the event time window between triggers. The original windowing, watermarking, triggering and persistence mechanisms are preserved, thus replicating the original processing and acting as flow control.

## References

- [1] How to beat the CAP theorem. http://nathanmarz.com/blog/how-to-beat-the-cap-theorem.html
- [2] Questioning the Lambda Architecture. https://www.oreilly.com/radar/questioning-the-lambda-architecture/
- [3] Designing a Production-Ready Kappa Architecture for Timely Data Stream Processing. https://eng.uber.com/kappa-architecture-data-stream-processing/
