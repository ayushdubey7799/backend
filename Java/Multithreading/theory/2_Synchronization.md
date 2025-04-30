# Synchronization
- Synchronization in Java is about controlling access to shared resources by multiple threads
  to prevent data inconsistency.
- Without synchronization, if two threads modify the same object at the same time, you can get:
    - Race conditions: Two users(threads) try to purchase the last product unit simultaneously.
    - Data corruption
    - Unexpected behavior


## Locks

### Intrinsic Locks (`synchronized` Keyword)
Every object has monitor lock | thread enters a synchronized method or block, it acquires that lock | when exits -> releases the lock.

Type                               | Syntax                                      | Lock Taken On
-----------------------------------------------------------------------------------------------------------------------
Synchronized Instance Method       | public synchronized void method() {}        | Lock on (this)
Synchronized Static Method         | public static synchronized void method() {} | Lock on Class object (ClassName.class)
Synchronized Block on Instance     | synchronized(this) {}                       | Lock on current object (this)
Synchronized Block on Other Object | synchronized(someObject) {}                 | Lock on specific object 

- If one thread locks a static method, other threads (on any instance) must wait for the lock.

### Reentrant locks (Java 5 - java.util.concurrent.locks.)
- More powerful and flexible locking mechanism compared to synchronized.
- Methods
    - lock.lock() -> Acquires lock.
    - lock.unlock() -> Must always be called (usually in finally block).
    - lock.tryLock() -> Try lock without waiting.
    - lock.lockInterruptibly() -> Interruptible lock acquisition 
    - (new ReentrantLock(true)) -> Fairness policy — First-come-first-serve locking.

## Difference between reentrant vs intrinsic synchronized
Aspect              | synchronized                      | ReentrantLock
--------------------------------------------------------------------------------------------------------
Locking             | Automatic                         | Manual (lock() and unlock())
Unlocking           | Handled automatically             | Must manually unlock (finally block important)
Flexibility         | Limited                           | High (tryLock, lockInterruptibly, fairness)
Interruptible       | No                                | Yes (lockInterruptibly())
Timeout             | No                                | Yes (tryLock(timeout))
Fairness            | No                                | Yes (fairness constructor)
Condition Variables | No                                | Yes (newCondition())
Performance         | Slightly better for simple cases  | Better for complex locking scenarios
    

- Use synchronized for simple cases.
- Use ReentrantLock when you need advanced control over locking.





