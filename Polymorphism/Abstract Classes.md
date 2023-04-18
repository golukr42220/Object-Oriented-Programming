**Pure Virtual Functions and Abstract Classes**

Defenition:-

Sometimes implementation of all function cannot be provided in a base class because we don’t know the implementation. Such a class is called abstract class. For example, let Shape be a base class. We cannot provide implementation of function draw() in Shape, but we know every derived class must have implementation of draw(). Similarly an Animal class doesn’t have implementation of move() (assuming that all animals move), but all animals must know how to move. We cannot create objects of abstract classes.
We've seen in the course different ways to achieve abstraction. We have also learned about abstracting the actual class of an object by its base class. There are certain use cases where there is no point in creating objects of the base class.

**Example**
```
Base Class: Animal
Derived Classes: Human, Lion, Snake, Fish, Eagle, etc.
```
**or**
```
Base Class: ElectronicDevice
Derived Classes: Phone, Laptop, Refrigerator, Television, etc.
```
**or**
```
Base Class: Employee
Derived Classes: IndividualContributor, Manager, CEO, etc.
```

Here, we would never want to create an object of the base class as the base class can't exist as a whole. It is always something which is a generalization of something whole/concrete (derived classes). Here, we would always create objects of the derived classes.

These types of classes where the only purpose of the class is to provide abstraction to the derived classes are known as abstract classes.

In case of abstract classes, many methods of the base class will be used as is through objects of the derived classes. If you notice carefully, there will be certain methods in the abstract classes which will always be overridden by the derived classes and would not have any implementation of its own.

Example
Animal will be an abstract class. There will be certain methods like move, speak, etc which won't have any implementation in the Animal class. What we can do is not even put these methods in the Animal class. What will be the problem if we do that?

The purpose of having an abstract class is to abstract out the actual class of the objects. We've previously learned that we cannot access the members of the derived class through a pointer of the base class unless that member has a definition in the base class as well. So, we will have to keep the method in the abstract class as well. We can anyway have an empty method definition in the base class like this:
```
virtual void move() {
}
```
- Note that the virtual keyword is important here as the method is supposed to be overridden.

As you can see that till now we have not put any restriction on the abstract class. Creating objects of an abstract class should not be allowed as it cannot exist as an actual whole entity but we are unable to restrict it right now. So it is not actually an abstract class right now.

C++ allows us to create an abstract class through pure virtual functions. Pure virtual functions are functions which have no implementation and should always be implemented by the derived classes. Didn't we already do that in the above code?

The above code still had an empty implementation. This is how we can rewrite using pure virtual functions:

```
virtual void move() = 0;
```

Here, move is a pure virtual function and the Animal class would become an abstract class. Now, if we try to create an object of Animal class, we would get an error because the function move won't be defined.

The syntax for creating a pure virtual function is:
```
class ClassName {
	.
	.
	.
	virtual return_type function_name(parameter_1, parameter_2, .....) = 0;
}
```

**Example**
Please go through the code properly and complete the tasks mentioned in the code comments.

**Expected Output**
```
Walk
Crawl
Fly
Walk
Fly
```

```
#include <bits/stdc++.h>
using namespace std;

class Animal {
public:
	void eat(string food) {
		cout << "Eating " << food;
	}

	virtual void move() = 0;
};

class Human: public Animal {
public:
	void move() {
		cout << "Walk";	
	}
};

class Snake: public Animal {
public:
	void move() {
		cout << "Crawl";	
	}
};

class Eagle: public Animal {
public:
	void move() {
		cout << "Fly";
	}
};

int main() {
	// your code goes here
	// 
	// Note that we cannot create Animal objects now as Animal is an abstract class
	// The following code will give error if uncommented.
	// Task: Uncomment the next line, run and see the error. Comment again after that.
	// Animal animal;

	Human human_1;
	Snake snake_1;
	Eagle eagle_1;
	Human human_2;
	Eagle eagle_2;

	//Creating an array of Animal pointers
	Animal *animals[5];

	//Assigned addresses of human_1 and eagle_1 to 0th and 1st index of animals
	animals[0] = &human_1;
	animals[1] = &snake_1;
	//Task: Assign addresses of eagle_1, human_2 and eagle_2 to 2nd, 3rd and 4th index of animals
	animals[2] = &eagle_1;
	animals[3] = &human_2;
	animals[4] = &eagle_2;
	
	//Task: Hit run and see what gets printed and if it is what you expected
	for (int i = 0; i < 5; i++) {
		animals[i]->move();
		cout << endl;
	}
	
	return 0;
}
```


**Some Interesting Facts: **

**1)** A class is abstract if it has at least one pure virtual function. 
**2)** We can have pointers and references of abstract class type.

```
#include<iostream>
using namespace std;
  
class Base
{
public:
    virtual void show() = 0;
};
  
class Derived: public Base
{
public:
    void show() { cout << "In Derived \n"; }
};
  
int main(void)
{
    Base *bp = new Derived();
    bp->show();
    return 0;
}
```
**Output:** 
```
In Derived
```

**3)** If we do not override the pure virtual function in derived class, then derived class also becomes abstract class.

```
#include<iostream>
using namespace std;
class Base
{
public:
    virtual void show() = 0;
};
  
class Derived : public Base { };
  
int main(void)
{
  Derived d;
  return 0;
}
```
`Compiler Error: cannot declare variable 'd' to be of abstract type 
'Derived'  because the following virtual functions are pure within
'Derived': virtual void Base::show()`

**4)** An abstract class can have constructors. 
The following program compiles and runs fine. 

```
#include<iostream>
using namespace std;
  
// An abstract class with constructor
class Base
{
protected:
int x;
public:
virtual void fun() = 0;
Base(int i) {
              x = i; 
            cout<<"Constructor of base called\n";
            }
};
  
class Derived: public Base
{
    int y;
public:
    Derived(int i, int j):Base(i) { y = j; }
    void fun() { cout << "x = " << x << ", y = " << y<<'\n'; }
};
  
int main(void)
{ 
    Derived d(4, 5); 
    d.fun();
    
  //object creation using pointer of base class
    Base *ptr=new Derived(6,7);
      ptr->fun();
    return 0;
}
```

**Output:**
```
Constructor of base called
x = 4, y = 5
Constructor of base called
x = 6, y = 7
```

**5)** An abstract class in C++ can also be defined using struct keyword.

E.g. :
```
struct shapeClass{
virtual void Draw()=0;
}
```







