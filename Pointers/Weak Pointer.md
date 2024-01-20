# Weak Pointer

A `weak_ptr` is a type of smart pointer in C++ that adds a level of indirection and safety to a raw pointer. It is mainly used to break reference cycles in cases where two objects have shared pointers to each other, or when you need a non-owning reference to an object that is managed by a `shared_ptr`.

A `weak_ptr` doesn’t increase the reference count of the object it points to, which is a crucial distinction between `weak_ptr` and `shared_ptr`. This ensures that the object will be deleted once the last `shared_ptr` that owns it goes out of scope, even if there are `weak_ptrs` still referencing it.

To use a w`eak_ptr`, you must convert it to a `shared_ptr` using the `lock()` function, which tries to create a new `shared_ptr` that shares ownership of the object. If successful, the object’s reference count is increased and you can use the returned `shared_ptr` to safely access the object.

Here’s an example of using `weak_ptr`:

```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    void DoSomething() {
        std::cout << "Doing something...\n";
    }
};

int main() {
    std::weak_ptr<MyClass> weak;

    {
        std::shared_ptr<MyClass> shared = std::make_shared<MyClass>();
        weak = shared;

        if(auto sharedFromWeak = weak.lock()) {
            sharedFromWeak->DoSomething(); // Safely use the object
            std::cout << "Shared uses count: " << sharedFromWeak.use_count() << '\n'; // 2
        }
    }

    // shared goes out of scope and the MyClass object is destroyed

    if(auto sharedFromWeak = weak.lock()) {
        // This block will not be executed because the object is destroyed
    }
    else {
        std::cout << "Object has been destroyed\n";
    }

    return 0;
}
```

In this example, we create a `shared_ptr` named shared that manages a MyClass object. By assigning it to a `weak_ptr` named weak, we store a non-owning reference to the object. Inside the inner scope, we create a new `shared_ptr` named `sharedFromWeak using weak.lock()` to safely use the object. After the inner scope, the MyClass object is destroyed since shared goes out of scope, and any further attempt to create a `shared_ptr` from weak will fail as the object is already destroyed.

**Explanation:**

1. An instance of `std::weak_ptr<MyClass>` named weak is declared.

2. Inside a local scope, a `std::shared_ptr<MyClass>` named shared is created using `std::make_shared<MyClass>()`. The weak pointer is assigned the value of shared.

3. An if block checks if the weak pointer can be converted to a `std::shared_ptr` using the `lock()` function. If successful, it means that the shared pointer is still valid. Inside the block, the DoSomething() method is called on the MyClass object, and the use count of the shared pointer is printed, which is `2`.

4. The shared pointer goes out of scope at the end of the local scope, and the reference count is decremented.

5. Outside the local scope, another if block checks if the weak pointer can be locked to a `std::shared_ptr`. However, since the shared pointer has gone out of scope, the `lock()` function returns an empty `std::shared_ptr`, and the else block is executed. The program prints "Object has been destroyed."

Output:

```
Doing something...
Shared uses count: 2
Object has been destroyed
```
This output demonstrates the safe usage of `std::weak_ptr` to prevent potential issues related to dangling pointers when the underlying `std::shared_ptr` goes out of scope.

[weak_ptr](https://en.cppreference.com/w/cpp/memory/weak_ptr)






