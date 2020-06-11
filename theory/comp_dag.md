# Parallelism of Computation DAG

A parallel program can be represented as a computation DAG where vertices are a series of computation and edges represent dependencies.

Some definitions for a computation DAG:
- `work`: the execution time on 1 core 
- `span`: the execution time on inf cores = the critical path of the DAG

## Parallelism and Speedup

Amdahl's law gives a loose upper bound of the parallelism: if a fraction `r` of a program must be run serially, the parallelism is at most `1/r`. This bound is too optimistic because it does not account for dependencies between tasks.

- Parallelism: `work / span` (i.e., average work per step along the critical path)
- Speedup on P cores: `work / T_P` where T_P is execution time on P cores

## Scheduling

### Greedy scheduling
Idea: do as much as possible on every step.

Theorem: `T_P <= work / P + span`

Corollary: Any greedy scheduler achieves within a factor of 2 of optimal.

### Work stealing
Idea: when any worker becomes idle, it steals ready tasks from another random worker.

Theorem: `T_P ~= work / P + O(span)`

---
## References

[MIT 6.172 Lecture 7](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-172-performance-engineering-of-software-systems-fall-2018/index.htm)

[Parallel Computing: Theory and Practice](http://www.cs.cmu.edu/afs/cs/academic/class/15210-f15/www/tapp.html)
