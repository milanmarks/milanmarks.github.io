---
title: 'MapReduce'
date: 2022-02-24
permalink: /posts/2022/MapReduce/
tags:
  - distributed system

---

[MapReduce](http://nil.csail.mit.edu/6.824/2021/papers/mapreduce.pdf) is a programming model for large data operating on multiple computers. Key functions listed below:

1. partitioning the input data
2. scheduling the program’s execution across a set of machines
3. handling machine failures
4. managing the required inter-machine communication

# model

1. *map*: input key/value pairs $\to$ intermediate key/value pairs 
2. *reduce*: values with same key->combine

# implementation

<img src="https://github.com/milanmarks/milanmarks.github.io/raw/master/images/mapreduce.PNG" alt="mapreduce" style="zoom: 50%;" />

* reduce worker sort the intermediate pairs
* master keep the state of the tasks (idle, in-progress, completed)


## Fault Tolerance

### worker failure

* master ping every worker periodically
* completed map task on a failure worker will be re-executed (result on local disk), complete reduce task will not (result on global disk)

### master failure

change master and continue after checkpoints

### semantics in the presence of failures

* a mask task produce $R$ result files, a reduce task produce one result
* weak semantics?

## locality

## task granularity

* map phase $M$ and reduce phase $R$ should be much greater than worker number
* $M$ to make individual task roughly 16 MB to 64 MB, $R$ a small multiple of worker 

## Backup tasks

# refinement

## combiner

* partial merging
* executed after *map* by the same worker

# performance

# within google