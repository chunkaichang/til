# Generate weighted random numbers

The following method generates an index randomly given the weights. 

Essentially, it builds the CDF of weights and then performs sampling.

```python
import random
import itertools
import bisect

def weighted_rng(weights):
    """Return the index to a random number given the weights (an iterable)

    e.g., if weights = [2,3,5]
    return 0, 1, 2 with probability of 0.2, 0.3, 0.5, respectively
    """
    totals = list(itertools.accumulate(weights))

    rnd = random.random() * totals[-1]

    # linear search
    for i, total in enumerate(totals):
        if rnd < total: return i

    # binary search using bisect (for many weights)
    ## return bisect.bisect_right(totals, rnd)
```

---
## Reference
[Eli Bendersky's website](https://eli.thegreenplace.net/2010/01/22/weighted-random-generation-in-python)
