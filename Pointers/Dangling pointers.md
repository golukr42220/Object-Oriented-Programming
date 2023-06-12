****Dangling pointers****

- A pointer pointing to a memory location that has been deleted (or freed) is called dangling pointer. There are three different ways where Pointer acts as dangling pointer

- 1. **De-allocation of memory**

```cpp
// Deallocating a memory pointed by ptr causes
// dangling pointer
 
#include <cstdlib>
#include <iostream>
 
int main()
{
    int* ptr = (int *)malloc(sizeof(int));
 
    // After below free call, ptr becomes a
    // dangling pointer
    free(ptr);
     
    // No more  dangling pointer
    ptr = NULL;
}
```

- 2. **Function Call **

```cpp
// The pointer pointing to local variable becomes
// dangling when local variable is not static.
 
#include <iostream>
   
int* fun()
{
    // x is local variable and goes out of
    // scope after an execution of fun() is
    // over.
    int x = 5;
   
    return &x;
}
   
// Driver Code
int main()
{
    int *p = fun();
    fflush(stdin);
   
    // p points to something which is not
    // valid anymore
    std::cout << *p;
   
    return 0;
}
```

- **The above problem doesn’t appear (or p doesn’t become dangling) if x is a static variable.**

```cpp
// The pointer pointing to local variable doesn't
// become dangling when local variable is static.
 
#include <iostream>
using namespace std;
 
int *fun()
{
    // x now has scope throughout the program
    static int x = 5;
 
    return &x;
}
 
int main()
{
    int *p = fun();
    fflush(stdin);
     
    // Not a dangling pointer as it points
    // to static variable.
    cout << *p;
     
    return 0;
}
```


```cpp
Variable goes out of scope 
void main()
{
   int *ptr;
   .....
   .....
   {
       int ch;
       ptr = &ch;
   } 
   .....   
   // Here ptr is dangling pointer
}
```

Indirection through- or deleting a dangling pointer will lead to undefined behavior. Consider the following program:

```cpp
#include <iostream>
int main()
{
    int* ptr{ new int }; // dynamically allocate an integer
    *ptr = 7; // put a value in that memory location

    delete ptr; // return the memory to the operating system.  ptr is now a dangling pointer.

    std::cout << *ptr; // Indirection through a dangling pointer will cause undefined behavior
    delete ptr; // trying to deallocate the memory again will also lead to undefined behavior.

    return 0;
}
```
In the above program, the value of 7 that was previously assigned to the allocated memory will probably still be there, but it’s possible that the value at that memory address could have changed.
It’s also possible the memory could be allocated to another application (or for the operating system’s own usage), and trying to access that memory will cause the operating system to shut the
program down.

- Deallocating memory may create multiple dangling pointers. Consider the following example
 
 ```cpp
 #include <iostream>
int main()
{
    int* ptr{ new int{} }; // dynamically allocate an integer
    int* otherPtr{ ptr }; // otherPtr is now pointed at that same memory location

    delete ptr; // return the memory to the operating system.  ptr and otherPtr are now dangling pointers.
    ptr = nullptr; // ptr is now a nullptr

    // however, otherPtr is still a dangling pointer!

    return 0;
}
```
  
There are a few best practices that can help here.

First, try to avoid having multiple pointers point at the same piece of dynamic memory. If this is not possible, be clear about which pointer “owns” the memory (and is responsible for deleting it) 
  and which pointers are just accessing it.

Second, when you delete a pointer, if that pointer is not going out of scope immediately afterward, set the pointer to nullptr. We’ll talk more about null pointers, and why they are useful in a bit.
  
-------------------------------------------------------------------------

****NULL Pointer****

**NULL** Pointer is a pointer which is pointing to nothing. In case, if we don’t have address to be assigned to a pointer, then we can simply use `NULL`. 

```cpp
#include <iostream>
using namespace std;
 
int main()
{
    // Null Pointer
    int *ptr = NULL;
     
    cout << "The value of ptr is " << ptr;
    return 0;
}
 ```
 
----------------------------------------------------------------------------------
 
 ****Wild pointer****
  
  A pointer that has not been initialized to anything (not even `NULL`) is known as `wild` pointer. The pointer may be initialized to a `non-NULL` garbage value that may not be a valid address. 

  ```cpp
int main()
{
    int *p;  /* wild pointer */
 
    int x = 10;
 
    // p is not a wild pointer now
    p = &x;
 
    return 0;
}
```
  
