****Introduction****

A virtual function is a member function defined in the base class, re-defined by a derived class. It is declared using the virtual keyword. It tells the compiler to perform late binding or dynamic linkage on function.

It uses the single pointer to refer to all the objects of the different classes, i.e., we create a pointer to the base class that refers to all the derived objects. But, when the base class pointer contains the address of the derived class object, it always executes a base class function. This issue can be resolved using a virtual function.

A virtual is a keyword preceding the standard declaration.

C++ determines which function is invoked at the runtime based on the type of the object pointed by the base class pointer when the function is made virtual.

**Rules of Virtual Function**

-  While declaring a virtual function, we follow some guidelines:

-  Virtual functions are accessed through object pointers, must be members of some class, and cannot be static members.
 
-  Virtual functions can be friends from other classes.
 
-  A virtual function should be defined inside the base class, even though it isn’t used.
 
-  The prototypes of a virtual function of the base class and all of the derived classes must be identical. If the two functions have the same call but different prototypes, C++ will consider them as overloaded functions.
 
-  We cannot have a virtual constructor, but we can have a virtual destructor.

`Let us look into an example of virtual function to get a better understanding.`

**Example**

```cpp
#include <iostream>
using namespace std;

class Base
{
	public:
	virtual void Output()
	{
		cout << "Output Base class" << endl;}
	void Display()
	{
		cout << "Display Base class" << endl;
	}
};

class Derived: public Base
{
	public:
	void Output()
	{
		cout << "Output Derived class" << endl;
	}
	void Display()
	{
		cout << "Display Derived class" << endl;
	}
};

int main()
{
	Base* bpointr;
	Derived dpointr;
	bpointr = &dpointr;
	bpointr->Output();
	bpointr->Display();
}
```
**Output**

```
Output Derived class
Display Base class
```


****Working of Virtual Functions****

**VTABLE**

The virtual table is a lookup table of functions used to solve function calls in a dynamic/late binding manner. Vtables are used for virtual functions. It is a short form for a virtual function table. It is a static table created by the compiler.

If a class contains virtual functions, then the compiler does two things:

- If an object of that class is created, then a virtual pointer is inserted as a data member of that class to point to the VTABLE of that class. A new virtual pointer is inserted as a class data member for every new object created.
 
- Irrespective of whether the object is created or not, the class contains a member of a static array of a function pointer called VTABLE while storing the address of each virtual function held in that class.
 
** Example**

```cpp
#include<iostream>
using namespace std;
class base {
public:
    void function_1() { cout << "base-1\n"; }
    virtual void function_2() { cout << "base-2\n"; }
    virtual void function_3() { cout << "base-3\n"; }
    virtual void function_4() { cout << "base-4\n"; }
}; 

class derived : public base {
public:
    void function_1() { cout << "derived-1\n"; }
    void function_2() { cout << "derived-2\n"; }
    void function_4(int x) { cout << "derived-4\n"; }
};

int main()
{
    base *p;
    derived obj1;
    p = &obj1;
    p->function_1();
    p->function_2();
    p->function_3();
    p->function_4();
    return 0;
}
```

**Output**

```
base-1
derived-2
base-3
base-4
```


****Frequently Asked Questions****

**1. What is a virtual function?**

A virtual function is defined in the base class, re-defined by a derived class.

**2. Can there be any virtual Destructors?**

Yes, but these destructors must be for the base class instead of the child class.

**3. Does every virtual function need to be overridden?**

No, it is not mandatory to re-define a virtual function.

**4. Can we have a constructor as Virtual?**

Constructors cannot be virtual because they want to be defined in the class.

-------------------------------------------------------------------------------

```cpp
#include<iostream>
using namespace std;

class A{

    public:
    virtual void function(){
        cout<< "A class function"<<endl; 
    }
};

class B:public A{

    public:
    void function(){
        cout<<"B class function"<<endl;
    }
};

int main(){

    A a;
    B b;
    a.function();
    b.function();

    A *a1=&b;
     a1->function();
}
```

**Output**
```
A class function
B class function
B class function
```

When we have `A` class pointer which is having the address of B class object `A *a1=&b`. When we write `a1->function()`, B class `function()` method will be called 
because we have written function in A class as `virtual function`. When compiler see virtual keyword before function. it bind at run time. 
At run time, memory of B class object will be created and stored in A class pointer.  

When compiler see  `a1->function()`. it will check the address a1 object having and called that class method. so a1 has address of B class object, so B class `function()`
will be called.

**How compiler know, which class function to call?**

Ans:- Using vtable(virtual table) and vptr(virtual pointer)
