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

