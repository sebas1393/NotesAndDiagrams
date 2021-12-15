<h1>Stack: </h1>

- Tracks methods calls.
- Contain Frames which hold parameters, return address and local variables for each method call.
- A stack frame is removed when returning from a method. All local variables go out of scope at this point.
- If you call too many methods, the stack will fill up completely and .Net throws an StackOverflow exception.
