#  Orchestrating Specknets

As we have multiple devices each with their own time keeping device; we need a method of syncing time across all of them

## Time Syncing
###  Local Specknets

For applications where the specks are nearby and can contact a "base station", we can use time division multiplexing to sync each reading with a master time step.  

!["TDMA Diagram"](/assets/tdma.png)

### Widly Distributed Specknets

For applications where the specks are spread over a large area; for example a weather monitoring system; we cannot easily connect to a "base station". Thankfully we can use GPS to track time; GPS provides a constant time via satelittes across the globe meaning all our specks will run of the same master time.

## Application Stack

A specknet will often have 3 layers; the lowest being sensors. These collect data, perform minimal processing and send it to a remote server. A remote server processes the data and performs intensive analysis. This is then made available to the client through a GUI. 

