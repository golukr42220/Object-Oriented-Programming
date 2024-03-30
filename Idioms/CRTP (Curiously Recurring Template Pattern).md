# CRTP (Curiously Recurring Template Pattern)

The Curiously Recurring Template Pattern (CRTP) is an advanced C++ programming technique that utilizes templates and inheritance to achieve static polymorphism. The basic idea behind CRTP is to define a base class template with a derived class that inherits from it, passing itself as a template argument to the base class. This allows the base class to refer to the derived class's type statically, enabling powerful compile-time optimizations and customization.

**Here's how CRTP works and its key components:**

**1. Base Class Template:**

The base class template defines functionality that depends on the derived class's type. It is usually parameterized by the derived class itself as a template argument.

**2. Derived Class:**

The derived class inherits from the base class template and provides its own implementation of certain methods. It passes itself as a template argument to the base class.

**3. Static Polymorphism:**
Since the base class is templated on the derived class's type, the compiler can optimize method calls statically, resulting in efficient code generation.

**Example with CRTP:**

Here's a simple example to illustrate the CRTP pattern:

```cpp
#include <iostream>

// Base class template using CRTP
template <typename Derived>
class Base {
public:
    void interface() {
        std::cout << "Calling implementation provided by derived class:" << std::endl;
        static_cast<Derived*>(this)->implementation();
    }

    void commonFunction() {
        std::cout << "Common function from base class" << std::endl;
    }
};

// Derived class 1 using CRTP
class DerivedClass1 : public Base<DerivedClass1> {
public:
    void implementation() {
        std::cout << "Implementation provided by DerivedClass1" << std::endl;
    }
};

// Derived class 2 using CRTP
class DerivedClass2 : public Base<DerivedClass2> {
public:
    void implementation() {
        std::cout << "Implementation provided by DerivedClass2" << std::endl;
    }
};

int main() {
    // Create objects of the derived classes
    DerivedClass1 obj1;
    DerivedClass2 obj2;

    // Call the interface function for DerivedClass1
    obj1.interface(); // Output: "Implementation provided by DerivedClass1"

    // Call the interface function for DerivedClass2
    obj2.interface(); // Output: "Implementation provided by DerivedClass2"

    // Call a common function from the base class for DerivedClass1
    obj1.commonFunction(); // Output: "Common function from base class"

    // Call a common function from the base class for DerivedClass2
    obj2.commonFunction(); // Output: "Common function from base class"

    return 0;
}
```

**Detailed Explanation:**

**Base Class Template (CRTP):**

- The `Base` class template is defined with a single template parameter, Derived, which represents the `derived` class inheriting from it.

- It provides an `interface()` method that calls the implementation() method provided by the derived class.

- Additionally, it includes a `commonFunction()` method that is common to all derived classes.
  
**Derived Classes:**

- Two derived classes, `DerivedClass1` and `DerivedClass2`, inherit from `Base<DerivedClass1>` and `Base<DerivedClass2>` respectively.

- Each derived class overrides the `implementation()` method with its own implementation.
  
**Main Function:**

- In the `main()` function, objects of `DerivedClass1` and `DerivedClass2` are created.

- The `interface()` method is called for each `object`, demonstrating static polymorphism as the correct `implementation()` method is invoked at compile-time.

- The `commonFunction()` method from the base class is also called for each `object`, showcasing code reuse through inheritance.
  
**Benefits of CRTP:**

- `Static Polymorphism:` CRTP enables `static polymorphism`, resulting in efficient method resolution at compile-time.

- `Code Reuse:` CRTP facilitates code reuse through `inheritance` and method `overriding`.

- `Compile-Time Safety:` Errors related to method signatures or type mismatches are detected at compile-time, enhancing code safety.
  
**Conclusion:**

CRTP is a powerful design pattern in C++ for achieving static polymorphism and code reuse through inheritance and templates. By leveraging compile-time optimization and type safety, CRTP enables efficient and flexible design patterns in modern C++ programming.

---------------------------------------------------------------------------------------------------------

How `static_cast<Derived*>(this)->implementation()` and `DerivedClass1 : public Base<DerivedClass1>` works ?


 Let's break down the line `static_cast<Derived*>(this)->implementation();` in the context of the Curiously Recurring Template Pattern (CRTP) example:


**1 static_cast<Derived*>(this):**

- This part of the expression casts the `this` pointer to a pointer of type `Derived*`.

- The `this` pointer represents a pointer to the current object, which is an instance of a derived class.

- By casting `this` to `Derived*`, we're essentially converting the `base` class pointer to a pointer of the `derived` class type. This allows us to access the member functions of the `derived` class through the `base` class pointer.

**2  ->implementation():**

- Once the `this` pointer is cast to `Derived*`, we use the `arrow operator (->)` to access the `implementation()` member function of the derived class.

- This calls the `implementation()` method provided by the derived class.

**Explanation:**

In the CRTP example, the `static_cast<Derived*>(this)` expression allows the `base` class template to access the `implementation()` method of the `derived` class. Here's how it works:

- When the `interface()` method of the `base` class template is called, it is invoked on an object of the `derived` class.

- Inside the `interface()` method, this refers to the `current object`, which is of the `base` class type `(Base<Derived>)`.

- By casting `this` to `Derived*`, we obtain a pointer to the derived class `(Derived)` from the pointer to the base class `(Base<Derived>)`.

- With the pointer to the derived class, we can then access the `implementation()` method provided by the derived class.
  
Thus, the line `static_cast<Derived*>(this)->implementation()` effectively calls the `implementation()` method of the `derived` class, achieving `dynamic polymorphism`.
This mechanism is crucial for implementing static polymorphism in CRTP, where the base class template is able to access methods of the derived class through static binding, resulting in efficient code generation and optimization by the compiler.

---------------------------------------------------------------------------------

Let's break down the line class `DerivedClass1 : public Base<DerivedClass1>` in the context of the Curiously Recurring Template Pattern (CRTP) example:

Explanation:
In the CRTP example, the line class `DerivedClass1 : public Base<DerivedClass1>` establishes a relationship between `DerivedClass1` and `Base<DerivedClass1>`. Here's how it works:

- `DerivedClass1` is defined as a class that inherits publicly from the `Base<DerivedClass1>` class template.

- By inheriting from `Base<DerivedClass1>`, `DerivedClass1` becomes a specialization of the Base class template with itself as the template argument.

- This creates a hierarchical relationship where `DerivedClass1` is the derived class and `Base<DerivedClass1>` is the base class.

- As a result, `DerivedClass1` inherits all the members (functions and variables) of `Base<DerivedClass1>` and can access them using the . and `-> operators`.

- Furthermore, because `DerivedClass1` is the derived class and is passed as a template argument to Base, the CRTP allows Base to statically refer to members of `DerivedClass1` through `static binding`, enabling powerful compile-time optimizations and customization.

In summary, class `DerivedClass1 : public Base<DerivedClass1>` establishes inheritance between `DerivedClass1` and `Base<DerivedClass1>`, enabling the use of CRTP to achieve static polymorphism and code reuse in the example code.

