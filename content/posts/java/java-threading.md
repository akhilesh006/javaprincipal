+++ 
draft = false
date = 2020-06-02T08:17:23+05:30
title = "Threading"
description = ""
slug = "" 
tags = []
categories = []
externalLink = ""
series = []
showthedate = true
+++

## Thread:

- Thread can be created by new Thread() only.

- Each object in Java is associated with a monitor, which a thread can lock or unlock. Only one thread at
a time may hold a lock on a monitor. Any other threads attempting to lock that
monitor are blocked until they can obtain a lock on that monitor.

![Thread](/threading.png)

- If execution of the body is ever completed, either normally or abruptly, an unlock action is
automatically performed on that same monitor.
- If the method is an instance method, it locks the monitor associated with the instance
for which it was invoked. 
If the method is static, it locks the monitor associated with the Class object that represents the class in which the method is
defined.

## Synchronization:

Synchronize can be achieved by
1. synchronized(object){} block
2. synchronized method
2. volatile keyword
3. using class of java.util.concurrent package. 
4. Collections.synchronizedList, synchronizedSet, synchronizedMap
5. Collection's Legacy class: Vector, Stack(subclass of Vector) HashTable, Properties( subclass of HashTable)

Note: Use ArrayDeque instead of Stack when synchronization isn't needed.

##### java.lang.IllegalMonitorStateException
1. If wait(), notify() or notifyAll() are not used inside synchronized block or synchronized method.
2. If wait(), notify() or notifyAll() are called on other object.
``` java 
    Object sync = new Object();
    synchronized (this) {
        sync.wait(100);
    } 
```

- notify() does not guarantee which thread will be removed from wait set.

### Wait Sets
- A Wait Sets is a set of thread.
- Each object have its own Wait set and Monitor.
- When a object is created, Its wait set is empty.
- Wait set are manipulated by Object.wait(), notify(), notifyAll() and reset when interrupted by a thread.
- wait(0) = wait(0,0) = wait()
- When a thread executing a wait() method of an object if, That thread is added into wait set of that object.
  and perform n unlock action on that object. Now thread does not execute any instruction until it is removed from wait set of that object.
- Thread can be removed from wait set by following action:
    - notify is called on that object.
    - notifyAll is called on that object.
    - An interrupt is performed on waiting thread.
    - Thread timeout, JVM internally remove the thread.


##### Q: What happens When notify() in interrupt() both are called simultaneously.
A: If notify() executed first then thread will be successfully removed from the wait set and a interrupt call will be placed for other thread waiting on that object.
   If interrupt() is executed first then thread will removed from wait set throwing InterruptedException, and wait() will be placed for other thread waiting on that object.

```java
Thread.currentThread(); //Return reference of current thread executing this line.
Thread.sleep(); // sleep and hold the lock.
Thread.dumpStack(); // Print stack trace, Used for debugging purpose.
Thread.yield(); // A hint to the scheduler that the current thread is willing to yield its current use of a processor. The scheduler is free to ignore this hint.

```

##### (§17.3) The compiler is not forced to flush write from cache to shared memory in register and read value from register.

```java
while (!this.done)
    Thread.sleep(1000);
```
The compiler is free to read the field this.done just once, and reuse the cached value in
each execution of the loop. This would mean that the loop would never terminate, even if
another thread changed the value of this.done.

#### data race (§17.4.5) Broken code: improperly synchronized
- there is a write in one thread,
- a read of the same variable by another thread,
- and the write and read are not ordered by synchronization.

| Thread 1    | Thread 2 |
| :----:      | :-------------: |
|  1. x = A   |      3. y = B       |
|  2. B = 1   |      4. A = 2       |

              or 

| Thread 1    | Thread 2 |
| :----:      | :-------------: |
|  1. B = 1   |     3. y = B       |
|  2. x = A   |     4. A = 2       |



