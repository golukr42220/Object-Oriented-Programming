
- ****Ambiguity Resolution in Inheritance****

[What Is the Diamond Problem in C++? How to Spot It and How to Fix It](https://www.makeuseof.com/what-is-diamond-problem-in-cpp/)

Ambiguity in inheritance can be defined as when one class is derived for two or more base classes then there are chances that the base classes have functions with the same name. So it will confuse derived class to choose from similar name functions. To solve this ambiguity scope resolution operator is used “::”. An example program is shown below to demonstrate the concept of ambiguity resolution in inheritance.

- ****Ambiguity in Multiple Inheritance****

Multiple Inheritance is another feature of C++ that a class can inherit from more than one class. For example, a derived class can be inherited from more than one base class or derived classes.

The most obvious error with Multiple Inheritance is Ambiguity errors, this occurs during function overriding. Do you want to learn how I can solve the Ambiguity problem in Multiple Inheritance? 
This question is also the same as how I can solve the Ambiguity problem in Class methods?

To explain this, first, let’s give an example. Suppose that we have two base classes ( Animal and Wing ), and they have the same function name ( info() ) which is not overridden in the derived 
class. If we try to call the function using the object of the derived class, the compiler will show us an Ambiguity error, because there is a conflict with two same names and the compiler doesn’t 
know which function to call. For example,

```cpp

#include <iostream>
class Animal // Base Class 1
{
  public:
 int weight;
 
 void info()
 {
 std::cout << "Weight:" << weight << "cm\n";
 }
};
 
class Wing // Base Class 2
{
  public:
 int wingwidth;
 void info()
 {
 std::cout << "Wing width:" << wingwidth << "cm\n";
 }
};
 
class Bird: public Animal, public Wing  // Derived Class 2
{
 
};
 
 
int main()
{
 Bird eagle;
 eagle.weight = 3;   // in kgf
 eagle.wingwidth = 280;  // in cm
 
 eagle.info();
 
 getchar();
 return 0;
 
}
```

If we run this code above in C++, we will face with Ambiguity Error as in here,

- C++ allows to use same function names in different classes, to avoid this error in Multiple Inheritance we should use their `class names with :: operator`. So first we should write object then 
dot with base `class name and :: operator` with the method name in that base class. See example below,

```cpp
eagle.Wing::info();
eagle.Animal::info();
```

So we can use both `info()` functions (methods) of two different bases in Multiple Inheritance. Full example will be as follows,

```cpp
#include <iostream>
 
class Animal // Base Class 1
{
  public:
 int weight;
 
 void info()
 {
 std::cout << "Weight:" << weight << "cm\n";
 }
};
 
class Wing // Base Class 2
{
  public:
 int wingwidth;
 void info()
 {
 std::cout << "Wing width:" << wingwidth << "cm\n";
 }
};
 
 
class Bird: public Animal, public Wing  // Derived Class 2
{
 
};
 
 
int main()
{
 Bird eagle;
 eagle.weight = 3;   // in kgf
 eagle.wingwidth = 280;  // in cm
 
 eagle.Wing::info();
 eagle.Animal::info();
 
 getchar();
 return 0;
}
```
