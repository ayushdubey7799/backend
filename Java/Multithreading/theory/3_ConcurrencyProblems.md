# Concurrency Problems

## Race Condition
- Two threads access a shared resource at the same time, outcome depends on order of execution.
- Result: Inconsistent state | Incorrect results | Hard-to-reproduce bugs
- Identify -> Operations that seem atomic but are actually multiple steps (read,modify,write).
- How to Avoid -> 
    - Synchronization (synchronized methods/blocks)
    - Atomic Variables (AtomicInteger, AtomicLong)
    - Higher-level concurrency classes (ConcurrentHashMap, CopyOnWriteArrayList)

*Atomicity + Visibility + Ordering — All three must be ensured to prevent race conditions.*


## Deadlock
- A deadlock occurs when two or more threads are waiting indefinitely for each other to
  release locks they need.
