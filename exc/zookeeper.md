# Zookeeper

## Motivation

Distributed systems need to be coordinated to operated efficiently, take configuration for example; if we wish to update the configuration of our system we must pass the new parameters to all machines while remaining available. 

A lock can help this, a lock provides mutually exclusive access to a machines critical resources. 

Previous approaches have developed services for each of the different coordination needs. This works well but requires lots of maintainable and is inflexible. 

Rather than develop an individual system for locking, Zookeeper provides an API that allows developers to implement their primitives. 
Zookeeper has non blocking API, meaning that the client does not have to wait for a command to complete before continuing execution. Zookeeper uses an event system to manage these callbacks. 

>_Aside_: Micro & Monolitic Kernals
> 
> In a monolithic kernal, everything is baked in; if an application needs to do a sys call, it can use a single kernal. This can yield better performance as it is optimized for one system. However this means that all code runs in the same privilage; meaning a single fault brings down the entire OS.
>
> In a micro kernal, the base kernal is the bare minimum and all other operations are built on top of them. This means that if there is a crash, it won't kill the entire OS.

## Overview

Zookeeper has some jargon to get over:
 
* _Client_ is a user of the ZooKeeper service
* _Server_ is a process providing the ZooKeeper Service
* _znode_ is a in-memory data node in the ZooKeeper data
* _session_ is established when a _client_ conencts to ZooKeeper and  obtain a _handle_ through which they issue requests. 

### Architecture

Zookeeper uses state machine replication to ensure that it is fault tolerant (see GFS). Wheen a client connects, they connect to a single server. 

### Z-Nodes

ZooKeeper abstracts a series of znodes into a hierarchical structure similar to UNIX file systems. There are two types of _znodes_ that a client can create:
* _Regular_: Created and removed explicitly by the user
* _Ephemeral_: Created explicitly and are moved by the user or are cleaned when the _session_ finishes.

The client can also use a sequential flag, nodes created with the sequential flag have a value set to a counter which is higher than all the other _znodes_ in its directory. 

ZooKeeper provides _watches_ to allow clients to receive timely notifications of changes without requiring polling. The watch command notifies the client when the information an operation returned changes. 

### Data Model

The data model in ZooKeeper is a hierarchal namespace. This is useful for finding subtrees and is natural to the developer as it work similarly to UNIX systems. 

However unlike files in file systems, _znodes_ don't actually store data, only metadata about which resources a client is using.

## API

| Name        | Parameters          |  Description                                                                                  |
|-------------|---------------------|-----------------------------------------------------------------------------------------------|
| Create      | `path, data, flags`   | Creates a new znode containing data, flags allow the type to be set (regular, ephemeral etc.) |
| Delete      | `path, version`       | Deletes znode if it exists at version                                                         |
| Exists      | `path, watch`         |  Returns if a znode exists, a watch provides a hook to watch the node                         |
| getData     | `path, watch`         | Returns the data and meta-data associated with a znode                                        |
| setData     | `path, data, version` | Writes data to the znode path                                                                 |
| getChildren | `path, data, version` |  Gets the children of a parent znode                                                          |
| sync        | `path`                | Waits for all update to propagate                                                             |

All methods can be performed asynchronously and synchronously. 

## Guarantees

ZooKeeper provides two ordering guarantees:

* **Linearizable Writes**: All requests that update the state are serializable and respect ordering given to them
* **FIFO client order**: All requests from a client are executed in first in, first out order. 
* **Wait Free**: Clients receive notifications of changes before the changed data become visible

>_Aside:_ Linearizability
> 
> An operation is linearizable if it given two operations S and T, the order of the operations; a, b, c is the same in S as it is in T. In the context of writing, this means that write a, will occur after b, after c _always_.

### Implementation

Zookeeper runs a set of servers, with a leader. The leader is elected and maintained for several seconds. It uses a state machine replication to tolerate faults. 

A zoo keeper node is stored in memory and logged to disk. 

A client connects to a Zookeeper node, this replica serves all read operations locally. If the client wants to write, the replica passes the request to the master. Which propagates it to the other replicas.; the leader serializes the operation and will ack it when a majority have completed the write.

> _Aside_: 2-Phase-Commit
>
> 2PC are used when transactions need to be carried out reliably in a leader-participant protocol.  
>
> The leader first sends a "prepare" message to all participants. The participants then try and log the write transaction and reply with an acknowledgement. When the leader has recieved a threshold number of acks, it will send a commit message to tell the paritipants to apply the changes.

### Example

A system comprised of a number of processes electes a leader to command  worker processes. When a new leader takes charge it must change a large amount of configuration parameters in all the worker processes and itself. For this we want:

* The workers to use the configuration that is being changed
* If the leader dies, we do not want teh processes to use the partial configuration. 

A distributed lock system, such as Chubby would enforce the first requirement, but not the second. With ZooKeeper, we can designate a path as the ready _znode_, meaning that other processes will only have the configuration once the leader is finished. 

## Example Usage

### Configuration Management
 
We can store a configuration in a znode `z_c`. When a process starts, it gets the configuration from `znode` using `getData` and specify a watch on it. WHen `z_c` is updated, all the processes are notified and they can re-get the configuration. 

### Rendezvous Points

If a clinet has a scheduled job, it may not always be clear where the workers or master will be on the network; however a worker may start before a master meaning it cannot simply read from a file. 

Zookeeper can provide a rendezvous point `z_r` to all workers and the master. On start up, a worker will get a watch on `z_r` and are notified when the master fills in the network info. Even better, if `z_r` is ephermial, then the node will be cleaned after execution!

### Locks

We can use a znode as a lock; to acquire a lock, a client tries to create a znode with the Ephemeral flag. If it succeeds, the client holds the lock; o/w the client can watch the node until it has been freed.

This suffers from the Herd Effect, where many clients will rush to get the next lock. 

To overcome this we line up clients using children znodes, these use the Sequential flag to order themselves. Clients now watch the n-1 node until it is deleted and check if they are the lowest remaining, when it is the lock is theirs. Ephemeral flags allow crashed processes to release their locks, preventing blockage. 
