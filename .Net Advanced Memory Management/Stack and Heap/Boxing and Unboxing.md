# Boxing and Unboxing
**Boxing / Unboxing** is a process that lets you use value types and reference 
types interchangeably in your code.

Each boxing operation creates **one new object** on the heap.
A boxed object occupies more memory than the original value type.

## Boxing:
Takes a value on the **Stack** and stores it as an object on the **Heap**. Boxing happens when you assign a value type
to a variable, parameter, field or property of type object.

## UnBoxing:
unpacks a boxed object on the **heap**, and copies the value type inside back to the **stack**. Unboxing happens when
you cast an object value to a value type.
	using System;

### Example:	
	namespace Boxing_Unboxing {
	
		class MainClass { 
			public static void Main(string[] args) { 
				int a = 123; 
				
				// Internaly .net box the primitive value on the heap.
				object b = a;
				
				// .Net unbox the object into a primitive value into the stack.
				int c = (int)b;
				
				Console.WriteLine(c);
				
			}
		}
	
	}


