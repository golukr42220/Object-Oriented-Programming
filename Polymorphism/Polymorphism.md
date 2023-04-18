**Polymorphism**

The word “polymorphism” means having many forms. In simple words, we can define polymorphism as the ability of a message to be displayed in more than one form. A real-life example of polymorphism is a person who at the same time can have different characteristics. A man at the same time is a father, a husband, and an employee. So the same person exhibits different behavior in different situations. This is called polymorphism. Polymorphism is considered one of the important features of Object-Oriented Programming.

**Types of Polymorphism**
- Compile-time Polymorphism
- Runtime Polymorphism

![image](https://user-images.githubusercontent.com/45598340/232893530-c30c9367-e0ed-4c79-8ade-e46ddf37370e.png)

**1. Compile-Time Polymorphism**
This type of polymorphism is achieved by function overloading or operator overloading.

**2. Runtime Polymorphism**

This type of polymorphism is achieved by Function Overriding. Late binding and dynamic polymorphism are other names for runtime polymorphism.

**How does the compiler perform runtime resolution?**

The compiler maintains two things to serve this purpose:

- [vtable](https://en.wikipedia.org/wiki/Virtual_method_table) : A table of function pointers, maintained per class. 
- [vptr](https://en.wikipedia.org/wiki/Virtual_method_table#Implementation): A pointer to vtable, maintained per object instance (see [this](https://www.geeksforgeeks.org/c-virtual-functions-question-12/) for an example).

![image](https://user-images.githubusercontent.com/45598340/232894844-dd70ce88-3a21-43f5-a1c9-44958afba701.png)

The compiler adds additional code at two places to maintain and use vptr.

1. Code in every constructor. This code sets the vptr of the object being created. This code sets vptr to point to the vtable of the class. 

2. Code with polymorphic function call (e.g. bp->show() in above code). Wherever a polymorphic call is made, the compiler inserts code to first look for vptr using a base class pointer or reference (In the above example, since the pointed or referred object is of a derived type, vptr of a derived class is accessed). Once vptr is fetched, vtable of derived class can be accessed. Using vtable, the address of the derived class function show() is accessed and called.

**Is this a standard way for implementation of run-time polymorphism in C++? **

The C++ standards do not mandate exactly how runtime polymorphism must be implemented, but compilers generally use minor variations on the same basic model.



