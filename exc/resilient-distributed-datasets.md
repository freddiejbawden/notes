# Resilient Distributed Dataset Paper Summary

## The Problem

Many frameworks need to repeatidly write large amounts of data to an external data store. This leads to a substantial overhead due to data replication, disk I/O and serialization.

Ideally we want to distribute our data set across several localized instances to reduce the file transfer time. Current solutions of this type use fine grained updates to mutable states (e.g. cells in a table). This means that the only way to replicate this across mulitple locations is through replication - which is very expensive. 

## RDDs

RDDs create an interface based on coarse-grained transformations (e.g. map, filter, join)  that apply the same operation to many data items. We then store the transformations rather than the data - sim to how modern VC works. 

If a RDD is lost, the RDD has information on how it was derived from other RDDs to recompute its state at failure.

### Benefits of an RDD

RDDs are designed specifically with fault tolerance in mind. Distributed shared memory (DSM) is _very_ abstract, making it hard to implement low level fault tolerance into clusters. 

Another benefit is that since RDDs are immutable, we can easily run backup copies of slow tasks. This is difficult with DSM as two agents would have to access the same location in memory.

RDDs can schedule tasks based on locality, boosting performance.




