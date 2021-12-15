# Finalizers in .Net
Finalizers are class methods that are called when objects are about to be cleaned up by the `Garbage Collector`.

Many small **short-lived** objects with finalizers are **_Bad_**, because the finalizer extends their lifetime
into gen:1.

## Important notes:
- Only add finalizers at the **edge** of an object graph.
- Finalizers should be never **_reference_** other objects.
- Finalizers should be **extremely short** and run **Very quickly**.


- Finalizers are called in a random order.
- Objects with finalizers end up in **generation 1** and sometimes in **_generation2_**.
- When a process ends, some finalizers **might not** get called because if the host process exits, anything remaining in
the finalization queue **After 4 seconds** is discarded.
