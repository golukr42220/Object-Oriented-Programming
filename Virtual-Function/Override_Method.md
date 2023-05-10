
**Function Overriding**

When it redefines a function of the base class in a derived class with the same signature i.e., name, return type, and parameter but with a different definition, it is called function overriding

[Function Overriding](https://www.scaler.com/topics/function-overriding-in-cpp/)

```cpp
#include<iostream>
using namespace std;

class A{
 
     public:
      void function(){
        cout<<"A class function"<<endl;
      }
};

class B:public A{

    public:
     void function(){  // function overridng
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

**Out Put**
```
A class function
B class function
A class function

```

When we call function using object. Compiler check the object type,like  'a' is object of A class. When we  write `a.fuction()` compiler 
will call A class `function()` method, because compiler bind at compile time. same for B class. But when we have `A` class pointer which 
is having the address of B class object `A *a1=&b`. When we write `a1->function()`, A class `function()` method will be called 
because compiler doesn't know at compile time, what address a1 is having. why?. because compiler doesn't create the memory for variable 
at compile time. when compiler see  `a1->function()`. it will check the type of a1 object. so a1 is object of A class, so A class `function()`
will be called. which is not correct because at run time `a1` pointes to `b` object, so we expect to call `B` class `function()`.


**How to solve this problem?**

Ans:- Using `virtual keyword` before function defination in base class. When compiler see the virtual keyword before function, compiler will bind at run time.
We will see in details in Virtual function.
