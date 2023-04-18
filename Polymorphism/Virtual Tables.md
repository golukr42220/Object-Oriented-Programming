**What does dynamic dispatch mean?**
In this context, dispatching just refers to the action of finding the right function to call. In the general case, when you define a method inside a class, the compiler will remember its definition and execute it every time a call to that method is encountered.

[Virtual method table](https://en.wikipedia.org/wiki/Virtual_method_table)

Consider the following example:

```
#include <iostream>

class A
{
public:
  void foo();
};

void A::foo()
{
  std::cout << "Hello this is foo" << std::endl;
}
```

Here, the compiler will create a routine for `foo()` and remember its address. This routine will be executed every time the compiler finds a call to` foo()` on an instance of A. Keep in mind that only one routine exists per class method, and is shared by all instances of the class. This process is known as static dispatch or early binding: the compiler knows which routine to execute during compilation.

**So, what do vtables have to do with all this?**
Well, there are cases where it is not possible for the compiler to know which routine to execute at compile time. This is the case, for instance, when we declare virtual functions:

```
#include <iostream>

class B
{
public:
  virtual void bar();
  virtual void qux();
};

void B::bar()
{
  std::cout << "This is B's implementation of bar" << std::endl;
}

void B::qux()
{
  std::cout << "This is B's implementation of qux" << std::endl;
}
```

The thing about virtual functions is that they can be overriden by subclasses:

```
class C : public B
{
public:
  void bar() override;
};

void C::bar()
{
  std::cout << "This is C's implementation of bar" << std::endl;
}
```

Now consider the following call to bar():
```
B* b = new C();
b->bar();
```

If we use static dispatch as above, the call b->bar() would execute B::bar(), since (from the point of view of the compiler) b points to an object of type B. This would be horribly wrong, off course, because b actually points to an object of type C and C::bar() should be called instead.

Hopefully you can see the problem by now: given that virtual functions can be redefined in subclasses, calls via pointers (or references) to a base type can not be dispatched at compile time. The compiler has to find the right function definition (i.e. the most specific one) at runtime. This process is called dynamic dispatch or late method binding.

**So, how do we implement dynamic dispatch?**

For every class that contains virtual functions, the compiler constructs a virtual table, a.k.a `vtable`. The vtable contains an entry for each virtual function accessible by the class and stores a pointer to its definition. Only the most specific function definition callable by the class is stored in the vtable. Entries in the vtable can point to either functions declared in the class itself (e.g. `C::bar()`), or virtual functions inherited from a base class (e.g.` C::qux()`).

In our example, the compiler will create the following virtual tables:

![image](https://user-images.githubusercontent.com/45598340/232764051-e0f188cd-c884-49f4-9bc4-7f77e3a1d0db.png)

The vtable of class B has two entries, one for each of the two virtual functions declared in B’s scope: `bar()` and `qux()`. Additionally, the `vtable` of B points to the local definition of functions, since they are the most specific (and only) from B’s point of view.

More interesting is C’s vtable. In this case, the entry for `bar()` points to own C’s implementation, given that it is more specific than `B::bar().` Since C doesn’t override qux(), its entry in the vtable points to B’s definition (the most specific definition).

Note that vtables exist at the class level, meaning there exists a single vtable per class, and is shared by all instances.


**Vpointers**
You might be thinking: vtables are cool and all, but how exactly do they solve the problem? When the compiler sees `b->bar()` in the example above, it will lookup B’s vtable for bar’s entry and follow the corresponding function pointer, right? We would still be calling `B::bar()` and not `C::bar()`…

Very true, I still need to tell the second part of the story: vpointers. Every time the compiler creates a vtable for a class, it adds an extra argument to it: a pointer to the corresponding virtual table, called the vpointer.

![image](https://user-images.githubusercontent.com/45598340/232764707-15029a16-acc9-4559-a7e5-f86ad7d697cb.png)

**Virtual Destructors**
By now it should also be clear why it is always a good idea to make destructors of base classes virtual. Since derived classes are often handled via base class references, declaring a non-virtual destructor will be dispatched statically, obfuscating the destructor of the derived class:
```
#include <iostream>

class Base
{
public:
  ~Base()
  {
    std::cout << "Destroying base" << std::endl;
  }
};

class Derived : public Base
{
public:
  Derived(int number)
  {
    some_resource_ = new int(number);
  }

  ~Derived()
  {
    std::cout << "Destroying derived" << std::endl;
    delete some_resource_;
  }

private:
  int* some_resource_;
};

int main()
{
  Base* p = new Derived(5);
  delete p;
}

```
This will output:
```
> Destroying base
```
Making Base’s destructor virtual will result in the expected behavior:
```
> Destroying derived
> Destroying base
```
**Wrapping up**
- Function overriding makes it impossible to dispatch virtual functions statically (at compile time)
- Dispatching of virtual functions needs to happen at runtime
- The virtual table method is a popular implementation of dynamic dispatch
- For every class that defines or inherits virtual functions the compiler creates a virtual table
- The virtual table stores a pointer to the most specific definition of each virtual function
- For every class that has a vtable, the compiler adds an extra member to the class: the vpointer
- The vpointer points to the corresponding vtable of the class
- Always declare desctructors of base classes as virtual
