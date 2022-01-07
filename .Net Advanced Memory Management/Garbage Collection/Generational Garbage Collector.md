[Back to Menu](../Menu.md)

# Generational Garbage Collection
Makes partitions to the Heap memory on generational Heaps.

The garbage collector takes the objects greater than 85K and places onto the Large Objects Heap (LOH).


- ### Heap 0 Gen: 
    frequent Collection.
- ### Heap 1 Gen:
    Infrequent Collection.
- ### Heap 2 Gen:
    Very Infrequent Collection.
- ### Large Object Heap:
    Very Infrequent Collection and no Compaction. (only have on Gen Collection).

[Back to Menu](../Menu.md)