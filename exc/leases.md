# Leases For Consistency

## Motivation

Caching in a distributed enviroment has a set of unique challenges; reducing some of its perrformance benefits for sample communication and host failure. Leases can help provide efficienrt consistent access to cached data in distributed systems. 

## Leases and Caching

We use fixed term leases to provide an effictive caching method. When we grab a file, we grab a lease on the file; we then invalidate our copy of the data. As we have fetched the data and hold exclusive rights over it; we can use our local copy for the duration of our lease. 