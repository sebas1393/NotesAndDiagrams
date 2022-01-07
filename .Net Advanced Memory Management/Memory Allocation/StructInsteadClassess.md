[Back to Menu](../Menu.md)

# Struct versus values
- Structs are **value types**, classes are reference types.
- Assigning structs **copies the value** of all internal fields.
- Structs can implement interface but you cannot **_inherit_**.
- Structs cannot have **instance field initializers**.
- Structs cannot have a **parameterless constructor**.
- Structs cannot have **finalizers**.

## Use cases for structs
- Use structs when the data you're storing **represents a single value**.
- Use structs if your **data size is very small (24 bytes or less)** and you're
going to create **thousands of millions** of instances.
- Use structs if your data **is inmutable**.
- Use structs if your data will not have to be **boxed** frequently.

In all other cases, classes are better alternative.

## Summary
- Structs are pretty **much faster** than classes because they don't have a default 
constructor cannot be **inherit**, and are not collected by the `Garbage Collector`.
- Structs only allocate heap memory for their internal fields and do not have the 16-byte overhead that objects have.
- In certain scenarios using structs instead of classes can **dramatically reduce** the memory footprint and runtime in your code.

[Back to Menu](../Menu.md)