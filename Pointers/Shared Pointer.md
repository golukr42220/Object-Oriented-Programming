# Shared Pointer

A shared_ptr is a type of smart pointer in C++ that allows multiple pointers to share ownership of a dynamically allocated object. The object will be automatically deallocated only when the last shared_ptr that points to it is destroyed.

When using a shared_ptr, the reference counter is automatically incremented every time a new pointer is created, and decremented when each pointer goes out of scope. Once the reference counter reaches zero, the system will clean up the memory.

Code Example
Here’s an example of how to use `shared_ptr`:

```cpp
#include <iostream>
#include <memory>

class MyClass {
public:
    MyClass() { std::cout << "Constructor is called." << std::endl; }
    ~MyClass() { std::cout << "Destructor is called." << std::endl; }
};

int main() {
    // create a shared pointer to manage the MyClass object
    std::shared_ptr<MyClass> ptr1(new MyClass());
    
    {
        // create another shared pointer and initialize it with the previously created pointer
        std::shared_ptr<MyClass> ptr2 = ptr1;

        std::cout << "Inside the inner scope." << std::endl;
        // both pointers share the same object, and the reference counter has been increased to 2
    }

    std::cout << "Outside the inner scope." << std::endl;
    // leaving the inner scope will destroy ptr2, and the reference counter is decremented to 1
    
    // the main function returns, ptr1 goes out of scope, and the reference counter becomes 0
    // this causes the MyClass object to be deleted and the destructor is called
}
```
```
Output:

Constructor is called.
Inside the inner scope.
Outside the inner scope.
Destructor is called.
```
In this example, `ptr1 and ptr2` share ownership of the same object. The object is only destroyed when both pointers go out of scope and the reference counter becomes zero.

## When to Use std::shared_ptr

std::shared_ptr is used when multiple ownership of an object is required. It ensures the object is destroyed only when the last shared_ptr owning it is destroyed or reset.

This is particularly useful in scenarios like:

1. Shared Resources: Multiple components share access to a resource (e.g., configuration data, thread-safe caches, in game where multiple player using same resource like image  or looking at vediew).
2. Cyclic Dependencies: Situations where objects may have back-and-forth references, but std::weak_ptr is used to break cycles.

Example: Shared Resource

**Scenario:** Imagine a game where multiple players can view and interact with the same game object, like a treasure chest. Each player should have access to the treasure chest as long as it's relevant. Once all players lose access, the chest should be destroyed automatically.

## When Not to Use std::shared_ptr

1. If only one owner is required, use `std::unique_ptr`.
2. For performance-critical scenarios, avoid shared ownership unless necessary, as reference counting in `std::shared_ptr` has overhead.
3. Be cautious of cyclic dependencies (e.g., two shared pointers referencing each other). Use `std::weak_ptr` to break such cycles.

## Handling Cyclic Dependencies with std::weak_ptr

A cyclic dependency occurs when two or more `std::shared_ptr` objects reference each other, forming a reference cycle. In such cases, the reference count will never drop to zero, causing a memory leak because the objects can't be destroyed.

Using `std::weak_ptr` solves this issue by breaking the cycle. A std::weak_ptr does not contribute to the reference count of the std::shared_ptr it observes, allowing proper cleanup.

## Example: Cyclic Dependency with Two std::shared_ptr

When two `std::shared_ptr` objects reference each other directly or indirectly, they create a cyclic dependency. This results in a memory leak, as the reference count for both objects never reaches zero, preventing their destruction.

Here’s an example to demonstrate this issue:

```cpp
#include <iostream>
#include <memory>
using namespace std;

class B; // Forward declaration

class A {
public:
    shared_ptr<B> b_ptr; // A holds a shared_ptr to B

    A() {
        cout << "A created" << endl;
    }

    ~A() {
        cout << "A destroyed" << endl;
    }
};

class B {
public:
    shared_ptr<A> a_ptr; // B holds a shared_ptr to A

    B() {
        cout << "B created" << endl;
    }

    ~B() {
        cout << "B destroyed" << endl;
    }
};

int main() {
    // Create two shared pointers referencing each other
    shared_ptr<A> a = make_shared<A>();
    shared_ptr<B> b = make_shared<B>();

    // Establish cyclic dependency
    a->b_ptr = b;
    b->a_ptr = a;

    // The program ends, but the objects are never destroyed
    cout << "Program ending" << endl;
    return 0;
}
```

### Problem: Memory Leak

`a` and `b` are `shared_ptr` objects managing instances of `A` and `B`.

1. When `a` is assigned `b_ptr` and `b` is assigned `a_ptr`, a cyclic reference is formed.

2. The reference count for A and B objects never drops to zero:
`a` references `b` via `b_ptr`.
`b` references `a` via `a_ptr`.

3. At the end of the program:
`a` and `b` go out of scope, but the objects they manage (`A` and `B`) are not destroyed because their reference counts are still non-zero.
This causes a memory leak.


## Solution: Use std::weak_ptr

To break the cycle, one of the references (usually the one representing the weaker relationship) should be a `std::weak_ptr`.  A `std::weak_ptr` does not contribute to the reference count, thus breaking the cycle.

Here’s the fixed code:

```cpp
#include <iostream>
#include <memory>
using namespace std;

class B; // Forward declaration

class A {
public:
    shared_ptr<B> b_ptr; // A holds a shared_ptr to B

    A() {
        cout << "A created" << endl;
    }

    ~A() {
        cout << "A destroyed" << endl;
    }
};

class B {
public:
    weak_ptr<A> a_ptr; // B holds a weak_ptr to A (breaking the cycle)

    B() {
        cout << "B created" << endl;
    }

    ~B() {
        cout << "B destroyed" << endl;
    }
};

int main() {
    // Create two shared pointers referencing each other
    shared_ptr<A> a = make_shared<A>();
    shared_ptr<B> b = make_shared<B>();

    // Establish a non-cyclic dependency
    a->b_ptr = b;
    b->a_ptr = a;

    // The program ends, and both objects are destroyed
    cout << "Program ending" << endl;
    return 0;
}
```
Explanation of the Fix

1. A still holds a shared_ptr to B, meaning A "owns" B.

2. B now holds a weak_ptr to A, meaning it observes A without contributing to its reference count.

3. When a and b go out of scope:
- The shared_ptr references to A and B are released.
- Since B holds only a weak_ptr to A, the reference count for A and B drops to zero, allowing both objects to be destroyed.

Key Takeaways

`. Cyclic Dependency:

- Happens when two std::shared_ptr objects reference each other directly or indirectly.
- Prevents the objects from being destroyed, causing memory leaks.

2. Breaking the Cycle:

- Use std::weak_ptr for the weaker relationship.
- std::weak_ptr avoids contributing to the reference count, breaking the cycle.

3. When to Use std::weak_ptr:

- In tree or graph structures with bidirectional relationships.
- Observer patterns where observers track subjects but do not own them.

## Accessing std::weak_ptr
A `std::weak_ptr` does not directly manage the lifetime of an object and cannot be dereferenced like a `std::shared_ptr`. To access the object, you need to convert the `std::weak_ptr` to a `std::shared_ptr` using the `.lock()` method.

If the object has already been destroyed, `.lock()` will return `nullptr`. This mechanism is used to safely access the object without extending its lifetime.


Example:

```cpp
#include <iostream>
#include <memory>
using namespace std;

class Resource {
public:
    string name;

    Resource(const string& name) : name(name) {
        cout << "Resource created: " << name << endl;
    }

    ~Resource() {
        cout << "Resource destroyed: " << name << endl;
    }

    void print() {
        cout << "Using Resource: " << name << endl;
    }
};

void useResource(weak_ptr<Resource> weakRes) {
    // Attempt to lock the weak_ptr to get a shared_ptr
    if (shared_ptr<Resource> sharedRes = weakRes.lock()) {
        // If the object still exists, use it
        sharedRes->print();
    } else {
        // If the object has been destroyed, handle gracefully
        cout << "Resource is no longer available." << endl;
    }
}

int main() {
    weak_ptr<Resource> weakRes;

    {
        // Create a shared_ptr managing the resource
        shared_ptr<Resource> res = make_shared<Resource>("MyResource");

        // Assign the weak_ptr to observe the resource
        weakRes = res;

        // Use the resource while it's alive
        useResource(weakRes);
    } // The shared_ptr goes out of scope here, destroying the resource

    // Try using the weak_ptr after the resource is destroyed
    useResource(weakRes);

    return 0;
}
```
Output:
```
Resource created: MyResource
Using Resource: MyResource
Resource destroyed: MyResource
Resource is no longer available.
```

Explanation

1. Creating the Resource:

- A `std::shared_ptr<Resource>` is created to manage the resource.
- A `std::weak_ptr` observes the resource but does not extend its lifetime.

2. Using `.lock()`:

- The `useResource()` function attempts to access the object by calling `.lock()` on the `std::weak_ptr`.
- If the object is still alive, .lock() returns a valid std::shared_ptr, and the object can be safely accessed.

3. Object Destruction:

- When the `std::shared_ptr` managing the resource goes out of scope, the resource is destroyed.
- The `std::weak_ptr` does not prevent the resource from being destroyed.

4. Safe Access:

After the resource is destroyed, .lock() returns nullptr. This allows the program to handle the situation gracefully without accessing invalid memory. 

## Use Cases for Accessing Weak Pointers

1. Observer Pattern:
- Observers use std::weak_ptr to monitor a subject without preventing its destruction.

2. Breaking Cyclic Dependencies:
- Avoid holding strong references in bidirectional relationships.

3. Cache or Pool Management:
- Use `std::weak_ptr` to access cached objects without keeping them alive unnecessarily.

  
