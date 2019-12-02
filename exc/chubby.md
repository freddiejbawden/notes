# Chubby

## Motivation

Chubby is used to manage a large group of loosely coupled distributed systems consisting of moderately large numbers of small machines. It provides a lock service, that allows its clients to synchronize their activies and agree on information about their environment. 

### Chubby in the wild

Chubby is used in GFS to appoint a master server, and in BigTable to manage access to the master node. 

## Structure

Chubby is made up of two main components; a server and a client library. 

A chubby cell contains a set of servers known as replicas, placed across racks to reduced correlated failure chance. The replicas use a distributed consensus protocol to elect a master. 

![](assets/chubby1.png)

Each replica contains a simple database, but the master initiates reads and writes to the database. Non-master nodes simply copy the master. 

Read requests are satisifed by the master alone and write requests are propogated through the consensus protocol to all replicas, being acked when a majority have responded.

### Locks and Sequencers 

Each chubby file and directory can act as a reader-writer lock; either one client handle may hold the lock in exclusive (writer) mode, or any number of clients handles may hold the lock in shared (reader) mode.

An issue in distributed systems is that clients holding locks can die; or could be taking a long time to respond. Thus we don't want to immiediatly give out a new lock when we don't hear back from a client.

We can handle this using a lock-delay that specifies a timeout for each request. 

Another solution is using a sequencer. A lock holder may request a sequencer. THis is a byte string that describes the state of the lock immediately after acquisition; it contains the name of the lock , the mode in which it was acquired and the generation number. The client then passes the sequencer to th servers, if iit expects the operation to be protected. The server is tests if the sequencer is valid if not it rejects the request. The generation number changes with each release and aquisition, meaning if slow clients try to access a re-locked asset, they will be rejected. 

## Caching and KeepAlive 

To prevent clients from repeatidly calling the Chubby cell, clients will use caching. The master node maintains a knowledge of a clients cache and can invalidate if needed. 

The burden on maintaining connection is put on the client through KeepAlive call. A client acks each master interaction using a KA call.

## Sessions

Using a combination of KA and leases we can establish a "connection" between the client and master. The client gives a lease to a  client, this means that the master is not allowed to unilaterally terminate the session. 

A client cannot be sure if a master is alive or not; clients use a smaller local lease timeout to trigger a panic mode if the master does not respond before the timeout.

## Master Failover

When a master dies, things can potentially go very wrong. As Chubby locks are advisory, it is still possible for two clients to attempt to grab a file at the same time. This could occur during a master failover; in the old master a client gets a lock on A, the master fails and starts up without the knowledge of the previous lock on A and issues another lease. This leaves two clients with the same lock. 

When a client's local timeout triggers, it considers that the master is jeoprodised. It then has a grace period before trying to reestablish a lock, this allows the old locks to die before new ones are established


