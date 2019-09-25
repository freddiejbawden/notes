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
* Write Once - 
* Available
* Namespace
* Concurrency