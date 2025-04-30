# Thread Basics
Understanding how threads behave internally and how scheduling works is key to writing safe, scalable, and predictable multithreaded applications.

## Thread
Smallest unit of execution, lightweight subprocess
- Multiple threads share: 
    - same memory (heap, method area), 
    - Same open files, sockets, database connections (unless handled separately)
- Own stack, program counter, and (local variables - naturally thread-safe).

## Thead vs Process
Aspect            | Thread                             | Process
------------------------------------------------------------------------------------------
Memory            | Shared among threads               | Separate for each process
Communication     | Easy via shared memory             | Inter-Process Communication (IPC)
Resource Overhead | Low                                | High
Failure Impact    | One thread can down entire process | Isolated process crash

- In a web server, each incoming HTTP request is often handled by a different thread (pooled),
  not a separate process.

## Thread life cycle
State           | Meaning
--------------------------------------------------------------------------------------
NEW             | Thread is created but not yet started
RUNNABLE        | Eligible to run but might not be running (depends on scheduler)
BLOCKED         | Waiting for a monitor lock (synchronized block)
WAITING         | Waiting indefinitely for another thread’s action
TIMED_WAITING   | Waiting for a specified period
TERMINATED      | Completed execution or aborted
 Daemon vs User Threads
User Thread: Threads you create normally; keeps JVM alive.

Daemon Thread:
- BLOCKED happens if two threads are trying to enter a synchronized block/method on the same
  object.

## Creating Threads (threadCreation.java)
- Extending Thread class or implementing runnable interface (preferred bcoz of multiple 
  inheritance)


## Daemon vs User Threads
- User Thread: Threads you create normally; keeps JVM alive.
- Daemon Thread: Background threads, like garbage collection, don't prevent JVM shutdown.
                 Garbage Collector, Timer threads, background tasks, auto-save services.

