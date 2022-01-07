[Back to Menu](../Menu.md)

# Can We Modify Strings Directly?
Home
C# Memory Tricks: Learn How To Master The Garbage Collector
Can We Modify Strings Directly?
Here's what we know about strings in C#:

- Strings are represented by the String class in the NET Framework. The class has a bunch of static- and instance methods to query and modify the string data.

- Strings are immutable - when you try to modify a string, the NET Framework actually creates a new string on the heap and leaves the original string unmodified. This speeds up string comparisons but slows down modifications.

- If you make a lot of modifications to a string, you should use the StringBuilder class instead.

The NET runtime stores string data in a block of memory on the heap. Each Unicode character in the string is stored as a 16-bit word. Strings use UTF-16 character encoding to represent Unicode characters above U+FFFF.

So here's an interesting question - is it possible in C# to directly modify the contents of a string, without going through the methods of the String class? It must be possible, because this is what the StringBuilder class is doing internally.

But if this is possible, then how does it affect string immutability? Is the framework really going to allocate an entirely new string on the heap as soon as I try to modify a single character? How would that even work?

Let's find out with a snippet of code, something like this:

    using System;

    public class Program
    {
        // declare a const string - can we modify it?
        public const string sentence = "The quick brown fox jumped over the lazy dog";
    
        static void Main(string[] args)
        {
            // stuff happens here
        }
    }
The question is, can I add code to the Main method that will modify the contents of sentence ? Will the compiler and/or the NET Framework allow me to make this change?

Let me add another question. Assume that it is possible to change the contents of the string in sentence . As you may know, strings are immutable in C#, meaning any change to a string automatically creates a new string on the heap while leaving the original string unmodified.

If I manage to somehow change the contents of sentence , will this also automatically create a new string on the heap? Or can I directly access the memory where the string is stored, and change the characters in-situ?

These are interesting questions to ask because they probe how the NET Framework implements string immutability and they expose the limitations on what we can do when accessing memory directly.

First, let's try to modify the contents of sentence  directly. To do this, I mark the entire method as unsafe and then use the fixed  keyword to obtain a pointer to the memory location where the string is stored. I store that pointer in  pSentence :

    static unsafe void Main(string[] args)
    {
        fixed (char* pSentence = sentence)
        {
        // report initial state
        Console.WriteLine($"The sentence is: {sentence}");
    
        // modify the string...
     
        // report final state
        Console.WriteLine($"The sentence is: {sentence}");
     
        Console.ReadLine();
        }
    }
Now that I have a character pointer to string memory, the actual modifying is super easy. I can’t modify pSentence  directly, but I can create a new variable p , loop through every character in the string, and overwrite each character with an asterisk, like this:

    // modify the string
    char* p = pSentence;
    for (var i = 0; i < sentence.Length; i++)
    {
    *p = '*';
    p++;
    }
So what do you think is going to happen? A compiler error? A runtime exception? Or a complete system crash?

Actually, everything is fine. Here’s the program output:

The sentence is: The quick brown fox jumped over the lazy dog
The sentence is: ********************************************
How about that? Using pointers, I can actually modify a const string variable. Everything works fine, provided I mark my method as unsafe, and use the fixed  keyword to pin the string in memory and obtain a pointer to the character data.

The reason this works is because everything is an object in C#. A string constant is just another instance of the string class floating around in memory. The const keyword prevents me from modifying the variable itself, so I cannot point sentence  to another string. But I can use a char pointer to change the contents of the string itself.

So we’ve established that it is actually possible to modify string constants in C#, using pointers. But how does this affect immutability? Did I modify the string in-situ, or did the pointer operation actually clone the string?

This is very easy to check. All I need to do is display the memory address of the string pointer before and after the modification, like this:

    static unsafe void Main(string[] args)
    {
        fixed (char* pSentence = sentence)
        {
        // report initial state
        Console.WriteLine($"The sentence is: {sentence}");
        Console.WriteLine($"The address of the sentence is: {(IntPtr)pSentence}");

         // modify the string
         char* p = pSentence;
         for (var i = 0; i < sentence.Length; i++)
         {
            *p = '*';
            p++;
         }
     
         // report final state
         Console.WriteLine($"The sentence is: {sentence}");
         Console.WriteLine($"The address of the sentence is: {(IntPtr)pSentence}");
        }
    }
I am casting the char pointer to an IntPtr , which is a 64-bit integer memory address that I can write directly to the console.

Now when I run the program, the output looks like this:

The sentence is: The quick brown fox jumped over the lazy dog
The address of the sentence is: 6643414812
The sentence is: ********************************************
The address of the sentence is: 6643414812
And look at that – the exact same memory address, before and after the modification.

So here’s what we discovered:

– You can directly modify string constants in C#, by using unsafe pointer operations.

– This modifies the string in-situ, which breaks string immutability.

String immutability is actually implemented in code, in every method that modifies a string. Calling Substring  or Replace  or Remove  simply creates a new modified string and leaves the original string unchanged. There’s no compiler or runtime magic going on to make strings immutable. It’s all manually implemented in the framework classes.

And here’s another aha moment for you: the StringBuilder  class is nothing more than a wrapper for a bunch of unsafe pointer operations that modify the string directly in memory. By using the StringBuilder you get the benefit of being able to modify strings directly, without having to mark your code as unsafe.

So both classes wrap a block of memory containing character data. The String  class enforces immutability, which allows you to very quickly compare strings and basically treats them as a value type. The StringBuilder  class uses pointers to directly modify the character data. This breaks immutability but lets you very quickly modify strings without flooding the heap with copies.

[Back to Menu](../Menu.md)
