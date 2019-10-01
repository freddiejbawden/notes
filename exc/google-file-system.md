# Data Management with Google File System (GFS)

## Motivation

With huge amounts of data, reading this data from a single data source will run into bandwidth limitations. 

To solve this we should use multiple disks; this gives `no. of disks*bandwidth` capacity!

## What is a Distributed File System? 
A DFS allows users to access and use data from multiple locations as if it were their own. It abstracts away all the complexity and provides a simple API for client interaction.

## Design Requirements
* Ability to store large files
* Scalable 
* Performant
* Write Once (append only) - we only append to memory we don't create a new block for each input.
* Available
* Namespace
* Concurrency - multiple reads at the same time = oof

## Interface

It runs on a cluster of machines, a client accesses it via a client/server model (a client might be a MapReduce job). 

GFS exposes two API, 

### Simple shell interface
* Allows for standard UNIX commands; mv, rm etc.. 
* Copy(From/To)Local copies from the client to the file system.
* Additional commands for Snapshots, Appends...

> Aside: Snapshots
> 
> We use snapshots to rollback changes, we take a snapshot of the current filesystem allowing for a development and master copies meaning changes are not destructive.
> 
> Snapshots are stored within GFS using the "copy on write" method. This initially stores just the metadata of the files however, once the original data is changed the data is stored at its current state. This saves on storage space as a snapshot may not have differences in every file. 

### Programming API
* Allows access from many languages making application development easy with GFS
* This API is not compatible with POSIX API as it only allows for appends

> Aside: _POSIX_
> 
> POSIX is a set of IEEE standards that define how programming interfaces should operate. This makes it easy to port to other POSIX systems. POSIX is one of the reasons why you can port UNIX programs across many different derivate. e.g.Linux & MacOSX.

## Architecture

We have a master node (NameNode) which stores the metadata about a set of distributed slaves (DataNode).

### Basic Functionality

When a file is added to GFS, it is split into various chunks. These chunks are divided up between Data Nodes and stored in their individual file system. 

Namenode stores the location of the chunks in the DFS. 

### Master Safety

The master can be a point of fault if implemented incorrectly, if the master dies the metadata could also be lost. 

To combat this, we replicate the data into a disk. We also normally use multiple NameNodes in case we have a catastrophic failure. One issue with this is that the others can be lagging behind due to the nodes being updated async meaning that when the switch occurs, we cannot guarantee that the data on the new master will be up to date. 

This was fixed later with the inclusion of State Machine Replication

> Possible Exam Question: "How can we improve the master's reliability?"  

### Read Operation

1. A client asks for a file through sending metadata of the desired chunk
2. The master node replies with the location of the node in the DFS.
3. The client pulls the file directly from the data node. 

This splits the operation into two paths, the control path (1 & 2) and the data path (3). 
