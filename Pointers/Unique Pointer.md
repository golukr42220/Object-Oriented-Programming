# Unique Pointer (unique_ptr)

`std::unique_ptr` is a smart pointer provided by the C++ Standard Library. It is a template class that is used for managing single objects or arrays.

`unique_ptr` works on the concept of exclusive ownership - meaning only one `unique_ptr` is allowed to own an object at a time. This ownership can be transferred or moved, but it cannot be shared or copied.

This concept helps to prevent issues like dangling pointers, reduce memory leaks, and eliminates the need for manual memory management. When the unique_ptr goes out of scope, it automatically deletes the object it owns.

Let’s take a look at some basic examples of using unique_ptr:

Creating a unique_ptr
```cpp
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> p1(new int(5)); // Initialize with pointer to a new integer
    std::unique_ptr<int> p2 = std::make_unique<int>(10); // Preferred method (C++14 onwards)

    std::cout << *p1 << ", " << *p2 << std::endl;
    return 0;
}
Transferring Ownership
#include <iostream>
#include <memory>

int main() {
    std::unique_ptr<int> p1(new int(5));

    std::unique_ptr<int> p2 = std::move(p1); // Ownership is transferred from p1 to p2

    if (p1) {
        std::cout << "p1 owns the object" << std::endl;
    } else if (p2) {
        std::cout << "p2 owns the object" << std::endl;
    }

    return 0;
}
```

**Using unique_ptr with Custom Deleters**

```cpp
#include <iostream>
#include <memory>

struct MyDeleter {
    void operator()(int* ptr) {
        std::cout << "Custom Deleter: Deleting pointer" << std::endl;
        delete ptr;
    }
};

int main() {
    std::unique_ptr<int, MyDeleter> p1(new int(5), MyDeleter());
    return 0; // Custom Deleter will be called when p1 goes out of scope
}
```

Remember that since unique_ptr has exclusive ownership, you cannot use it when you need shared access to an object. For such cases, you can use std::shared_ptr


## Explanation of above code

This C++ code demonstrates the use of a custom deleter with a std::unique_ptr. Let's break it down step by step:

```cpp
struct MyDeleter {
    void operator()(int* ptr) {
        std::cout << "Custom Deleter: Deleting pointer" << std::endl;
        delete ptr;
    }
};
```
Here, a structure MyDeleter is defined. This structure has an overloaded `operator()` function, which serves as the custom deleter for the `std::unique_ptr`. The operator() function takes a pointer to an integer (int* ptr) and deletes it using the delete operator. Additionally, it prints a message to the standard output to indicate that the custom deleter is being called.

```cpp
int main() {
    std::unique_ptr<int, MyDeleter> p1(new int(5), MyDeleter());
```

In the main function, a std::unique_ptr named p1 is declared. It is templated with two types: int (the type of the managed object) and `MyDeleter` (the type of the custom deleter). An instance of `MyDeleter` is also provided to the constructor of the `std::unique_ptr` as an argument.

A dynamic memory allocation is performed using new int(5), and the resulting pointer is passed to the std::unique_ptr. The custom deleter is specified explicitly with MyDeleter().

```cpp
    return 0; // Custom Deleter will be called when p1 goes out of scope
}
```

The program returns 0, ending the main function. When `p1` goes out of scope at the end of the main function, the custom deleter `(MyDeleter::operator())` will be automatically called. This is a result of the `std::unique_ptr` being designed to automatically invoke its associated deleter when it goes out of scope. The custom deleter's message will be printed to the standard output, indicating the deletion of the dynamically allocated integer.

