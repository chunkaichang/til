# Implementing Mutual Exclusion

There are two ways to implement mutual exclusion for critical sections: with locks and without locks. Implementation with locks is easier but incurs high performance overhead, while implementation without locks may result in high performance but requires changes at a higher level (e.g., the algorithmic level).

## Implementation of Mutual Exclusion Locks (Mutexes)

Types of mutexes:
- Spinning: keeps trying until acquiring the lock (wasting processor cycles)
- Yielding: yields control to OS if the lock is not free.

### x86

The implementation relies on the fact that atomic instructions also implicitly act as memory fences.

> All assembly code uses AT&T syntax

- Spinning locks:

```assembly
Spin_Mutex:
    cmp 0, mutex        ; test if mutex is free
    je Get_Mutex
    pause               ; hint to hw that this is a spin-wait loop
                        ; avoid hw to execute the following code speculatively
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

### Performance issues
If the thread holding the lock is descheduled by the OS, the other threads cannot make progress either.

---

## Implementation Mutual Exclusion without Locks
There are well-known algorithms for lock-free mutual exclusion such as Dekker's algorithm and Peterson's algorithm which use only LOADs and STOREs. However, they assume sequential consistency (SC). Since SC hits performance (e.g., load bypassing is not allowed), modern processors implement more relaxed memory models (e.g., TSO, weak memory consistency, etc.). As a result, these algorithms need to use atomic loads/stores when running on modern machines.

Instead of using the above generic algorithms that mimic locks, applications can also adopt lock-free algorithms and data structures. These approaches allow threads to make progress without locks by using atomic instructions (e.g., compare-and-swap) or hw support for transactional memory.
