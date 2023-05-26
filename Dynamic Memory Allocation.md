
****Dynamic Memory Allocation****

[Dynamic Memory Allocation](https://www.scaler.com/topics/cpp/dynamic-memory-allocation-in-cpp/)

The process of allocating or de-allocating a block of memory during the execution of a program is called Dynamic Memory Allocation. The operators new and delete are utilized for dynamic memory 
allocation in C++ language, new operator is used to allocate a memory block, and delete operator is used to de-allocate a memory block which is allocated by using new operator.

**Types of Memory Allocation**

**1. Static Memory Allocation in C++ (Compile-time Memory Allocation)**
- When memory is allocated at compile-time, it is referred to as Static Memory Allocation.
- A fixed space is allocated for the local variables, function calls, and local statements, that can not be changed during the execution of the program.
- We can not allocate or de-allocate a memory block once the execution of the program starts.
- We can't re-use the static memory while the program is running. As a result, it is less effective.

**2. Dynamic Memory Allocation in C++ (Run-time Memory Allocation)**
- When memory is allocated or de-allocated during run-time, it is referred to as Dynamic Memory Allocation in C++.
- A variable space is allocated that can be changed during the execution of the program.
- We use dynamic/heap memory to allocate and de-allocate a block of memory during the execution of the program using new and delete operators.
- We can re-use our heap memory during the run-time of our program. As a result, it is highly effective.

****Dynamic Memory Allocation & De-allocation Criteria****

**1. Creating the Dynamic Space in Memory.**
During the dynamic memory allocation in C++, first, we have to create a dynamic space (in the heap memory). We use the new operator to create a dynamic space.

**2. Storing its Address in a Pointer**
Once a dynamic space a created, we have to store the address of the allocated space in a pointer variable to access and modify the contents of the memory block.

**3. Deleting the Allocated Space**
Once the user does not require the memory block, we delete the allocated space using the delete operator to free up the heap memory.

****The "new" operator in C++****
The `new` operator in C++ is used to dynamically allocate a block of memory and store its address in a pointer variable during the execution of a C++ program if enough memory is available in
the system.

**Syntax for "new" operator:**
```
data_type* ptr_var = new data_type;
```

`ptr_var` is a pointer, which stores the address of the type `data_type`.
Any pre-defined data types, like `int, char,` etc., or other `user-defined` data types, like `classes`, can be used as the data_type with the `new` operator.

**Example C++ Program:**

// how to allocate memory using the new operator example

```cpp
#include <iostream>
using namespace std;

int main() 
{
    // dynamically allocating an integer memory block
    int* ptr = new int;

    // storing a value at the memory pointed by ptr
    *ptr = 12;

    cout << "Value at memory pointed by ptr= " << *ptr;

    return 0;
}
```
 
**Output:**
```
Value at memory pointed by ptr= 12
```

**Explanation:**

A memory block is allocated using the `new` operator and the address of the memory block has been stored in the `ptr` pointer.
(`*ptr`) represents the value at the allocated memory location. We have assigned `12` in the memory using `*ptr = 12`expression.

****Initializing Dynamic Memory****

```
Syntax:
data_type* ptr_var = new data-type(value);
```

**Example**

```cpp
#include <iostream>
using namespace std;

int main() 
{
    // dynamically allocating an integer memory block
    // with 12 as its initial value
    int* ptr = new int(12);

    cout << "Value at memory pointed by ptr= " << *ptr;

    return 0;
}
```

**Output:**
```
Value at memory pointed by ptr= 12
```

- **What Happens if the System's Memory Runs out During the Execution of the Program?**
When there is insufficient memory in the heap segment during the run-time of a C++ program, the new request for allocation fails by throwing an exception of type `std::bad alloc`. 
It can be avoided using a `nothrow` argument with the `new` operator. When we use `nothrow` with `new`, it returns a `NULL` pointer. So, the pointer variable should be checked that is formed 
by `new` before utilizing it in a program.

**Example:**
```cpp
int* ptr = new(nothrow) int;

if (ptr == NULL)
{
   cout << "Allocation Failed!\n";
}
```

- ****The "delete" Operator in C++****
The `delete` operator is used to de-allocate the block of memory, which is dynamically allocated using the `new` operator. Since, a programmer must de-allocate a block of memory, once it is 
not required in the program. So, we have to use the `delete` operator to avoid memory leaks and program crash errors, which occur due to the exhaustion of the `system's memory`.

**Syntax:**

Syntax to delete a single block of memory:
```cpp
delete ptr_var;
```

**Syntax to delete an array of memory:**
```cpp
delete [] ptr_var;
```

**Example**
```cpp
#include <iostream>

using namespace std;

int main() 
{
    // allocating a new int stars variable using new
    int* stars = new int;
    *stars = 5000;

    cout<<"Visible stars in the sky: "<<*stars;

    // stars memory deallocated using the delete operator
    delete stars;

    cout<<"\nGarbage value: "<<*stars;

    stars = NULL;

    return 0;
}
```

**Output:**
```
Visible stars in the sky: 5000
Garbage value: 1491762488
```
(output garbage value is compiler dependent and will be different at each run)

**Explanation:**

- A memory block for the `stars` pointer is allocated using the `new` operator and the address of the memory block has been stored in the `stars` pointer.
- We have used the `delete` operator to de-allocate the memory pointed by the `stars` pointer, as it is no longer required in our program.
- The memory is de-allocated but the stars pointer is still pointing to some `garbage` location in the memory. So, it will show `garbage` value if printed on the output screen.
We have assigned `NULL` to the `stars` pointer in the end to avoid the situation of a  [dangling pointer](https://www.scaler.com/topics/c/dangling-pointer-in-c/).

![image](https://github.com/golukr42220/Object-Oriented-Programming/assets/45598340/8f055ed8-6060-40bc-8bb7-f2f0921bc982)


