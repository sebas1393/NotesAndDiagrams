[Back to Menu](../Menu.md)

# The Dispose Pattern
The **Dispose Pattern** provides a method for explicitly 
releasing scarce resources.
The `using` statement will automatically call the dispose 
method at the end of the using block.

- The **dispose pattern** dramatically **reduces** the length of time 
that resources are held by objects.
- Calling the dispose method suppresses the finalizer, which **prevents** 
the object's lifetime extending into gen:1.

[Back to Menu](../Menu.md)