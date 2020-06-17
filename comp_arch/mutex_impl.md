# Implementing Mutual Exclusion

There are two ways to implement mutual exclusion for critical sections: with locks and without locks. Implementation with locks is easier but incurs high performance overhead, while implementation without locks may result in high performance but requires changes at a higher level (e.g., the algorithmic level).

## Implementation of Mutual Exclusion Locks (Mutexes)

Types of mutexes:
- Spinning: keeps trying until acquiring the lock (wasting processor cycles)
- Yielding: yields control to OS if the lock is not free.

The following summarizes example implementation for x86 and RISC-V. For implementation at a higher level, Erik Rigtorp has written a [post](https://rigtorp.se/spinlock/) on how to implement spinlocks correctly in C++.

### x86

Here is an example implementation from MIT 6.172 lecture slides.
The implementation relies on the fact that atomic instructions also implicitly act as memory fences.

> The following assembly code uses AT&T syntax

- Spinning locks:

```assembly
Spin_Mutex:
    cmp 0, mutex        ; test if mutex is free
    je Get_Mutex
    pause               ; tell cpu that we are spinning
                        ; save power or allow other hw thread to proceed (if any)
    jmp Spin_Mutex
Get_Mutex:
    mov 1, %rax
    xchg mutex, %rax    ; try to acquire mutex
    cmp 0, %rax         ; test if successful
    jne Spin_Mutex

    <critical-section code>

    mov 0, mutex        ; release mutex
```

The key here is `xchg` (atomic exchange). The code between `Spin_Mutex` and `Get_Mutex` is not necessary for correctness but for performance (avoiding cache invalidation).

- Yielding locks:
Replaces the `pause` instruction above with yield call.

### RISC-V

Here is an example implementation from the RISC-V spec.

```assembly
    # a0 holds the lock address
    li t0, 1
again:
    lw t1, (a0)
    bnez t1, again
    amoswap.w.aq t1, t0, (a0)
    bnez t1, again

    <critical-section code>

    amoswap.w.rl x0,x0, (a0)
```

The `amoswap` is the atomic swap instruction. However, since RISC-V implements release consistency, it allows atomic instructions to act as more flexible memory fences via the aquire and release bits, and hence allows more optimization in hw or at compile time.

### Performance issues
- If the thread holding the lock is descheduled by the OS, the other threads cannot make progress either.
- For high performance, we need to consider cache coherence and implement the test-test-and-set idiom.

---

## Implementation Mutual Exclusion without Locks
There are well-known algorithms for lock-free mutual exclusion such as Dekker's algorithm and Peterson's algorithm which use only LOADs and STOREs. However, they assume sequential consistency (SC). Since SC hits performance (e.g., load bypassing is not allowed), modern processors implement more relaxed memory models (e.g., TSO, release consistency, etc.). As a result, these algorithms need to use atomic loads/stores when running on modern machines.

Instead of using the above generic algorithms that mimic locks, applications can also adopt lock-free algorithms and data structures. These approaches allow threads to make progress without locks by using atomic instructions (e.g., compare-and-swap) or hw support for transactional memory.


---
## References
[MIT 6.172](https://ocw.mit.edu/courses/electrical-engineering-and-computer-science/6-172-performance-engineering-of-software-systems-fall-2018/index.htm)
[RISC-V unprivileged spec](https://riscv.org/specifications/isa-spec-pdf/)
