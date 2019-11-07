# Containers

It can be difficult to isolate parallel apps on the same VM as they share the same memory. It can also be hard to run multiple applications optimally if they require different depencenies e.g. Java 8 vs 9. 

While single stack servers were okay for the early web, web apps have become much more complex; requiring multiple DBs, APIs and background workers. All of these can have different dependencies and configurations! 

This could be solved using a single hypervisor with serveral kernal stacks on top, however this is very wasteful for tiny services! A container usese the same kernal for all applications meaning multiple containers can be run on the same VM with low cost. 

## Overview

Containers are based on the UNIX `chroot` command; this provides a new root directory for a process and its children. This "jails" a process, disallowing it from accessing other files

![](chroot.png)

Chroot is a good solution however it doesn't allow for control over the resources allocated to each process; nor does it allow for the joining of several processes under one name space. For this we use `cgroups`. A `cgroup` is a limited allocation of resources that can be used by processes.

## Docker

### Why Docker?

* Docker instances are highly portable
* They are lightweight which boosts speed and footprint
* Docker Hub provides preconfigured instances
* It allows for the highly reliable microservice architecture. 

### How Docker? 

Docker runs using chroot and cgroups which ar abstracted away with a management interface. This interface sis on top of the kernel.

**Dockerfiles** specifies instructions to create a docker image, this could include depencencies or OS types. Dockerfiles can be pushed to your host, which takes the configuration and starts up a **container** using it.

A **container** has an **image**; a read only file system, and a series of **volumes** (shared memory).

### Images

Docker images are generally very small compared to other VM type applications. This is because docker images can share fundamental parts of the contains between containers. For example if many containers in the host ned to run python, they can all share the same python executables. This means that the only thing that is shared the small user defined R/W layer. 

### Pros and Cons

Docker provides:
* A faster boot time
* A lower memory and disk footprint
* Lower runtime overheaed
* More efficent deployment

However it has the side effects of 
* Weak isolation due to shared resources
* No cross containerisation

### Security Concerns
Due to the bad isolation, if you have a bad configuration a container can reaech out of its container to attack others

If the host kernal is vulnerable, all the docker instances beecome vulnerable as well, this is not a concern with VMs as they have their own kernal

Relying on external ecosystems means that untrusteed or outdated code could make it onto your container




