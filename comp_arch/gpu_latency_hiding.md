# Understanding Latency Hiding on GPUs
This is a reading note of [Understanding Latency Hiding on GPUs](https://www2.eecs.berkeley.edu/Pubs/TechRpts/2016/EECS-2016-143.pdf) by Vasily Volkov.


## Definitions
- `occupancy`: number of warps executed at the same time
- `warp throughput`: total number of executed warps / total execution time
- `mean warp latency`: the difference between warp start time and end time

These metrics are directly related via Little's Law. For example, we can know the needed occupancy to reach maximum throughput by computing the product of warp throughput and warp latency.

One way to visualize these metrics is to plot the start time and end time of each warp where x axis is time and y axis is warp id (see Fig 3.3).

## Modeling warp throughput

### Relationship between warp throughput and occupancy
This work proposes a model between warp throughput and occupancy (Fig. 3.4):

```
throughput ~= min( occupancy / latency bound, throughput bound)
```

where `latency bound` is a lower bound of warp execution time and `throughput bound` is an upper bound of warp throughput. Both depend on code and hardware.

This model has two modes. When occupancy is small, throughput is linear to occupancy and depends on latency. This is the latency-bound mode. On the other hand, when occupancy is large, throughput is limited by the machine's throughput. This is the throughput-bound mode.

### Issues of the model
1. In reality, there is a *gradual saturation effect* where latency-bound mode transitions to throughput-bound mode nonlinearly (e.g., due to resource sharing among warps, if one warp is issued, the others' latency increases).
2. Throughput can even decrease with occupancy (e.g., due to cache thrashing).
The latency bound (i.e., the minimum warp latency) is derived from the dependency graph of a kernel where nodes are instructions and edges are various dependencies (data dependencies, reissue latency of a warp, etc.). The latency bound is the critical path.

### Estimating throughput bound
The throughput bound is the minimum throughput among various pipeline stages (e.g., issue, execute, memory, etc.). The memory subsystem is usually the bottleneck. To derive the throughput of memory subsystem, we need (1) peak throughput per SM (in B/cycle) and (2) the amount of per-warp data transfer of the code (e.g., if the code has 3 transactions and each is 128B, then total amount is 128*3 B/warp).

```
throughput bound ~= peak memory throughput per SM / the amount of data transfer
```

## Understanding latency hiding
The above model can help understand latency hiding. For example, given a target kernel and a hardware model, the goal is to know the needed occupancy to reach max throughput via latency hiding.

This thesis found:
1. Although hiding memory latency requires more warps than hiding arithmetic latency, the ratio is decreasing over time due to the memory wall (i.e, improvement in memory throughput lags behind improement in compute).
2. If memory accesses are not coalesced, the needed occupancy is lower than the case where accessess are fully coalesced. This is because each warp poses higher pressure (more memory transactions per warp) on the memory subsystem.
3. The maximum of needed occupancy occurs when the workload is both compute- and memory-bound (in particular when arithmetic intensity equals the ratio of compute throughput to memory throughput).
