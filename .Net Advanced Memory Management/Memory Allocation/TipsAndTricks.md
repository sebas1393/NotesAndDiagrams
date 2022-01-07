[Back to Menu](../Menu.md)

# Tips and Tricks about Memory Allocation Optimizations

## Avoid Boxing and UnBoxing

- Avoid boxing and unboxing wherever you can.
    - Boxing floods the heap with lots of small objects and puts considerable pressure on the `Garbage Collector`.
    - Boxing and Unboxing should be avoided wherever posible.
  

## Do not concatenate strings
String objects are immutable, they cannot be changed, so when doing concatenations, the system internally creates new small objects.

- **Wrong Solution:** The concat operation of strings creates new small objects.

        using System;
        using System.Text;

        namespace ConsoleApp1 {
        class Program
        {
            private const int REPETITIONS = 100000;
            public static void StringTest()
            { 
                string result = string.Empty;
                for (int i = 0; i < REPETITIONS; i++)
                {
                     result += "#";
                }
            }
             static void Main(string[] args)
             {
                  StringBuilderTest();
             }
        }
        }



- **Correct Solution:** The StringBuilder is a character buffer of a certain capacity,that directly modify the original array of characters Append function avoid creating new objects.
When using a un-settled StringBuilder capacity .Net creates a default object capacity, when the object capacity it's full, it creates a new object with Twice his size.
In order to avoid the creation of new objects inside the loop, initialize the capacity of any list including StringBuilders.

        using System;
        using System.Text;

        namespace ConsoleApp1 {
        class Program
        {
            private const int REPETITIONS = 100000;
            public static void StringBuilderTest()
            {
                 StringBuilder result = new StringBuilder(REPETITIONS);
                 for (int i = 0; i < REPETITIONS; i++)
                 {
                      result.Append('#');
                 }
             }
             static void Main(string[] args)
             {
                  StringBuilderTest();
             }
        }
        }

## Golden Rule
- **String** methods never modify the original string. Instead they always make a copy, modify the copy, and return the copy as a result.
- Strings are optimized for fast comparisons, the **StringBuilder class** is optimized for fast modifications.
- **Always use StringBuilders** in mission criticalCode.
- In my Test Program switching from  string to StringBuilder reduced memory footprint by 99.9%.


## Always pre-size collections

## Golden Rule

Every Collection and list in .NET starts out with a certain **default capacity**,
and when it runs out of space it will create a new list of **twice the size** and 
copy everything over.

## Default collection capacity
- ArrayList: 4 items.
- Queue: 4 items.
- Stack: 16 items.
- List: 4 items.
- Dictionary: 12 Items.

So If you add hundreds or thousand of items to a collection without a pre-sized capacity, it will 
have to resize many times to accommodate all items.

## Avoid Calling ToList method in LINQ expressions

- Calling the `ToList()` method at LINQ expressions inflate the memory footprint in your code.
, and it's not good for the `Garbage Collector`.
- To fix this problem, call `ToList()` method **before running** the LINQ query.

Following this rule, we could optimize the memory usage by x25 don't forget it. 


[Back to Menu](../Menu.md)