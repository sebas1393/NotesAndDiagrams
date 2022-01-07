[Back to Menu](../Menu.md)

#Garbage Collection Process in **_.NET_**:

### Mark
Mark every object that references some value.
### Sweep
Remove All objects that aren't marked.
### Compact
Reallocate objects at the spaces that the removed objects left.


## Problems
- A mark and sweep Garbage Collector has to mark all live objects on the heap during each collection cycle.
- This can lead to long **Program Freezes** while the collection is running.
- Also: repeatedly marking the same objects over and over in each cycle is very **inefficient**.
## Solution: 
Generational garbage collector.

[Back to Menu](../Menu.md)