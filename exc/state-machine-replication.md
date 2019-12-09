# State Machine Replication

A SM in this context isn't as simple as a finite automata. We construct a state machine out of several state variables and commands which transform its state. A very simple example of a state machine in this context is a mutex lock.

```
mutex: state_machine
  var user: 
    client_id init Φ;
    waiting: list of client_id init Φ; 
  acquire: command
    if user = Φ -> 
      send OK to client;
      user := client
    if user != Φ -> 
      waiting := waiting ● client
  end acquire
  release: command
    if waiting = Φ ->
      user := Φ
    if waiting != Φ ->
      send OK to head(waiting);
      user := head(waiting)
      waiting := tail(waiting)
  end release
end mutex
```

Here we set out a very simple program which has infinite states (the queue can contain infinite client_ids) however can be encoded using the mutex state machine.

A state machine replication system must use 2F+1 replicas to ensure that if F machines fail, then a majority can assert their failure.

To allow a state machine to operate across several machines, we need to assert two factors;

* Agreement: Every nonfaulty state machine replica receives every request.
* Order: every nonfaulty state amchine replica processes the requests it recieves in the same relative order. 

## Enforcing Order

We must ensure that when a series of instructions arrives at each processer, the order in which they are executed is the same. There are several methods for achieving this; 

* Logical Clocks; use a hash that maps events to integers such that if `e` occurs before `e'` then `F(e) < F(e')`.

* Real time clocks: we can synchronize clocks across our replicas to provide ordering to events. We have our clocks synchronized to a degree smaller than the minimum message time. 

## Voting Tolerance 

To check if a node has failed we must vote; if the voter aggregator is faulty then we cannot trust the output. To combat this each replica calculates the outcome of the vote, this is then read by an external reader. 