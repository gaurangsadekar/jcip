# Chapter 2: Thread Safety

Writing thread safe code is about managing **shared state**. Shared means a variable could be accessed by multiple threads.
* Always a good principle to make your code right, then make it fast.

### Definition of Thread Safety:
A class is thread safe when it continues to behave correctly when accessed from multiple threads, regardless of the scheduling or interleaving of the execution of those threads by the runtime environment, and with no additional synchronization or other coordination on the part of the calling code.

Since the actions of a thread accessing a stateless object cannot affect the actions of another thread accessing the object, stateless objects are thread safe.

### Atomicity and Race Conditions:
The possibility of incorrect results being produced because of unlucky timing is called a race condition.

A race condition occurs when the correctness of a computation depends on the relative timing or interleaving of multiple threads by the runtime.
Most common race condition is a "check-then-act", where a potentially stale observation is used to make a decision on what to do next. For a check-then-act race condition,
- You observe something to be true
- then take action based on that observation
- but in fact the observation could have become invalid between the time you observed it and the time you acted on it, causing a problem

Another common race condition is the "read-modify-write", where you define the transformation of an object's state in terms of its previous state.

2 operations are atomic wrt each other, if from the perspective of the thread executing one, the other is either completely executed or not at all. An atomic operation is atomic wrt all operations, including itself, that operate on the same state. Including itself because it can be executed again and again.

Race conditions can be solved by making the accesses to the objects that are raced for atomic, but this works only if the state variables are independent.
To preserve state consistency of related state variables, update all related state in a single atomic operation.

### Locking:
#### Intrinsic Locking:
Java provides intrinsic locking using the `synchronized` block. Intrinsic locks in Java act as mutexes, which means that at most one thread may own the lock.

No thread executing in a synchronized block can observe another thread to be in the middle of a synchronized block guarded by the same lock.

#### Re-entrancy:
When a thread requests a lock that is already held by another thread, the requesting thread blocks.
Reentrancy means that locks are acquired on a per-thread basis rather than a per-invocation basis. Reentrancy is implemented by associating with each lock an acquisition count and an owning thread. Reentrancy facilitates encapsulation of locking behavior.

Reentrancy basically enables a thread holding a lock to request the same lock again, and that lock is granted to the thread.

### Guarding State with Locks:
For each mutable state variable that may be accessed by more than one thread, all accesses to that variable must be performed with the same lock held. Then we say that the variable is guarded by the lock.

A lock doesn't prevent the object from being accessed by other threads, it only prevents any other thread from acquiring the same lock.

### Liveness and Performance:

