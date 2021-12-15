# Heap

- The **new** keyword creates an object onto the heap.
- When variables on the stack go out of scope, their corresponding objects on the heap are **de-referenced** not destroyed.
- The **.Net** framework eventually starts a process called `Garbage Collection` and deallocates all de-referenced objects on the heap.