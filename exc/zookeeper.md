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

### Example

A system comprised of a number of processes electes a leader to command worker processes. When a new leader takes charge it must change a large amount of configuration parameters in all the worker processes and itself. For this we want:

* The workers to use the configuration that is being changed
* If the leader dies, we do not want teh processes to use the partial configuration. 

A distributed lock system, such as Chubby would enforce the first requirement, but not the second. With ZooKeeper, we can designate a path as the ready _znode_, meaning that other processes will only have the configuration once the leader is finished. 



