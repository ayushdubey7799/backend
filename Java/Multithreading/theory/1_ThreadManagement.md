# Thread Management in java


- start(): create new thread
- run(): call run() directly in current thread
- join():  wait for a thread to finish
- sleep():  pause thread temporarily
- yield():  hint to scheduler to pause thread
- setPriority():  hint priority to scheduler
- interrupt():  gracefully signal thread to stop


## Best Practices
Practice                                          | Why Important
---------------------------------------------------------------------------------------
Never call Thread.stop() or suspend()             | Unsafe, deprecated
Always handle InterruptedException properly       | For graceful shutdown
Prefer ExecutorService to manual thread management| Clean shutdown, thread pooling
Don't rely on thread priority for correctness     | It’s just a hint, not a rule
Always use start() instead of run()               | Avoid accidental serial execution

## start() vs run()
- start() requests the JVM to create a new thread and then calls the thread's run() method internally.
- run() just runs the method on the current thread like a normal method call.

## Thread class all methods

### Thread Lifecycle
start(), run(), join(), getState(), isAlive()

### Thread Control
sleep(), yield(), interrupt(), isInterrupted(), interrupted()
### Thread Metadata
setName(), getName(), getId(), setPriority(), getPriority()
### Daemon Threads
setDaemon(), isDaemon()
### Static Utilities
currentThread(), 
holdsLock(Object) - Tests if current thread holds the lock on a given object., 
dumpStack() - Prints current thread's stack trace.
