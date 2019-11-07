# Engineering at Scale

Autocompletion can seem simple, the common "trie" data structure will allow for quick look up of related strings. However to take the next step in autocompletion; providing personalization and localization adds a new level of complexity due to the number of dimensions.

## Bing Architecture Overview

Bing is constantly updating and changing its page rankings and data about the user to be ready to provide a quick response. This is done using Distributed ML and Map Reduce.

Using this offline model, when a user sends a query, the ranker can pull from the suggestion trie, a users history and context specific suggestions to do with a users interests or locations.

## Efficiently Modelling User Context

### Location

By modelling location based on search density rather than trying to divide up a location, we can learn natural divides in user location - for example the dense and diverse city of New York will have more location sensitive results e.g. based on boroughs, than rural Utah. 

## Trade Offs

No EXC solution is better than another, when creating a solution you must weight up the cost of complexity, stroage, compute usage and serving cost. 

To help tune these, we measure production systems, profile code and gate performance during builds to prevent degredation of performance over time. 

## System vs Fundamentals 

Its important to consider how code could be expanded in future, while its good to write very complex code - if no one can understand it, it's not worth using.

It is also important to consider how the impact introducing a new system propagates through the rest of the pipeline. If your new algorithm requires a new output format,make sure that can be done quickly first !

Often a service is larger than most engineers can reason about, its impossible to keep track of everything going on in the system. We need to balance abstraction and complexity to  allow engineers to build clever solutions quickly. 