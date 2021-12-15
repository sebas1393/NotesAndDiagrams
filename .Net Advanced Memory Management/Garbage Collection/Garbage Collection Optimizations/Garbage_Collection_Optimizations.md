# Garbage Collector Optimizations:
## Resume
- **Limit** the number of objects you create.
- Allocate, use, and discard **small** objects as fast as possible.
- **Re-use** all large objects.
- Use only _**small short**_-lived objects, and _**large long**_-lived objects.
- **Increase** lifetime or **decrease** size of **_large short_**-lived objects.
- **Decrease** lifetime or **Increase** size of **_small long_**-lived objects.
## Gen 0 Optimizations:
The more objects in generation 0, the more work the `Garbage Collector` has to do so.

- **Limit** the number of objects you create.
- Allocate, use, and discard objects as _**quickly**_ as possible.

### Example:
- The string concatenation creates objects on the memory, so doing concatenations 
inside the for loop it's creating pretty much objects in the memory:

  - **Wrong Solution:** Strings are immutable, both the ToString() and the concat operator, creates new objects on the heap so it's creating 2 objects for each iteration.
    
        StringBuilder s = new StringBuilder();
  
        for (int i=0; i < 10000; i++) {
            s.Append(i.ToString() + "KB");
        }
  - **Correct Solution:** Appending the strings with the `.Append()` method of the `StringBuilder` Class doesn't create new objects.
  
        StringBuilder s = new StringBuilder();

        for (int i=0; i < 10000; i++) {
            s.Append(i);
            s.Append("KB");
        }
- Listing objects on an ArrayList, creates an object foreach value, so instead of doing a generic ArrayList, you should create a strongly typed list like `List<int>` that creates an internal integer array.
  - **Wrong Solution:** ArrayList box their members on the heap, duplicating the value of the integers.
        
          ArrayList list = new ArrayList();

          for(int i = 0; i< 10000; i++){
              list.Add(i);
          }
  - **Correct Solution:** The List<int> don't box a new reference to each int value because he the reference knows it's an integer.

         List<int> list = new List<int>();

          for(int i = 0; i< 10000; i++){
              list.Add(i);
          }
  - **Wrong Solution:** Making Allocation and use of the object pretty far away, causes that the garbage collector to level up objects to another Gen
  so this could cause that the garbage collector do not delete the object even if it is not required.

        public static MyObject obj = new MyObject();
        //***************
        // Lots of code here
        //***************
        useMyObject(obj);
  - **Correct Solution:** Doing allocation, use and removing the reference with the `null` reference, makes that the garbage collector delete the object as sooner as possible.
        
        //***************
        public MyObject obj = new MyObject();
        useMyObject(obj);
        obj = null; // Indicates to the Garbage collector
        // Lots of code
        //***************
        

## LifeTime Optimizations
  - Object Pooling Strategy: re-use objects. Don't create a new object if you can use the existing one.
## Examples:
  
  - Short-lived objects should be re-use, so the `Garbage Collector` marks those as long-lived object and do the appropriate usage of memory for them.
    - **_Wrong Solution_**: The word **new** creates a new memory instance, so it's using two short-lived objects with the same purpose.

          ArrayList list = new ArrayList(8754);
          UseTheList(list);
          ...
          List = new ArrayList(85123);
          UseTheList(list);
          
    - **_Correct Solution_**: The **clear()** method of a list, clean the values and the references of the actual object and 
    can be re-used, so the `Garbage Collector` can mark the object as long-lived.
          
            ArrayList list = new ArrayList(12391);
            UseTheList(1234);
            ...
            List.Clear();
            UseTheList(1234);
  - **Reallocate after use**. Refactor code; The creation of tiny objects inside a large.
    - **_Wrong Solution_**: When the big list is created it's allocated on the Generational Heap, but when the tiny pairs
    it's being created they are allocated on the **Gen 0** heap and threaten as _short-lived_ object.
  
          ArrayList list = new ArrayList(87561);
    
          for (int i = 0; i < 10000; i++) {
          
            list.Add(new Pair(i, i+1));    
    
          }
      - **_Correct Solution_**: Instead of create one big List, we could create 2 big int[] list, and they are going to be
      allocated on the Long Object Heap, so the `Garbage Collector` it's going to reduce the number of check's about that
      objects.
    
            int[] list1 = new int[85761];
            int[] list2 = new int[85761];
          
            for (int i = 0; i < 10000; i++) {

              list1[i] = i;
              List2[i] = i+1;
            }
  
## Size Optimizations
The Garbage Collector assume 90% of all small objects are short-lived, and all large objects are long-lived, so:
  - **_Avoid_** _large_ short-lived objects.
  - **_Avoid_** _small_ long-lived objects.

###Strategies: 
- Split objects or reduce footprint.

### Examples:
  - Reducing footprint of objects changes the heap in which it's going to be stored so:
    - **_Wrong Solution_**: Creating an `int[]` to store buffers creates a four-byte space per each int stored in the 
    list, so instead of create 32768 values in the memory it's going to create (32768 * 4) objects on the memory, so
    the `Garbage Collector` it's going to store this object on the **Gen 2** heap.
    
          int[] buffer = new int[32768];
          
          for (int i = 0; i < buffer.Length; i++){
            buffer[i] = GetByte(i);
          } 
      
      - **_Correct Solution_**: `byte[32768]` arrays reduce the memory footprint instead of the `int[]` array so it's going to 
      use only 32768 bytes on the memory.
    
            byte[] buffer = new byte[32768];
            
            for(int i = 0; i < buffer.Length; i++ ){
              buffer[i] = GetByte(i);
            }
  - Merge Objects or re-size list to get larger objects.
    - **_Wrong Solution:_** When initializing a list without capacity, the system assign the default value (16K), but it
    could be dynamic, so the object it's going to be assigned to the default heap.
    
          Public static ArrayList list = new ArrayList();
          ...
          //Lots of other code here
          UseThisList(list);
    - **_Correct Solution:_** When the capacity it's explicit assigned (85190), the `Garbage collector` assign the object
    directly on the correct heap.
  
          public static ArrayList list = new ArrayList(85190);
          ...
          // lots of other code.
          ...
          UseThisList(list);

