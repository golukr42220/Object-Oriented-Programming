# Function Hiding in C++

Function hiding occurs in C++ when a derived class function with the same name as a base class function hides all the base class versions of that function, regardless of their parameter list (even if they are overloads).

This behavior is different from function overloading, where functions in the same scope can have the same name but different parameters.

To access the hidden base class functions, we explicitly need to use the scope resolution operator (::).

```cpp
#include <iostream>
using namespace std;

class Base {
public:
    void display() {
        cout << "Base class display()" << endl;
    }

    void display(int x) {
        cout << "Base class display(int): " << x << endl;
    }
};

class Derived : public Base {
public:
    void display() { // This hides all versions of 'display' in the base class
        cout << "Derived class display()" << endl;
    }
};

int main() {
    Derived obj;
    
    obj.display(); // Calls Derived's display
    // obj.display(10); // Error: no matching function for call to 'display(int)'

    obj.Base::display(); // Calls Base's display()
    obj.Base::display(10); // Calls Base's display(int)
    
    return 0;
}
```
Output:
```
Derived class display()
Base class display()
Base class display(int): 10
```

### Explanation

1. In the above example, the Derived class defines a function `display()` that hides all versions of `display()` in the Base class.

2. When `obj.display()` is called, it uses the Derived class's version.

3. If you attempt to call `obj.display(10)`, it results in a compilation error because the Derived class has hidden the base class functions, and the Derived class does not have a matching function with an int parameter.
   
4. To access the base class functions, you explicitly use `Base::display()`.

### Key Points

1. Function hiding happens when a derived class function with the same name hides all base class functions with that name.

2. Overloaded base class functions are also hidden.

3. To access the hidden base class functions, you need to use the base class name and scope resolution operator.

## When to Use Function Hiding in C++

Function hiding is not typically used as a design goal but can occur as a side effect of overriding functions in derived classes. However, there are scenarios where function hiding can be useful or is employed intentionally:

### 1. Redefining Behavior for Derived Class

- When you want a derived class to have its specific version of a function and you don't want the derived class to inherit or expose the base class's overloaded versions of that function.
- This is useful when the derived class has a narrower or different focus and the base class's functions are not relevant in the derived context.

### 2. Customizing Overloading in Derived Class

- If the derived class has different needs for function overloading, it may choose to hide the base class overloads entirely.
- This can happen when the parameters of the base class overloads are not applicable or when the derived class intends to define its own overloads.

###  3. Preventing Base Class Functions from Being Called Accidentally

- If certain base class functions are not intended to be used in the derived class, you can hide them by defining a function with the same name in the derived class.
- This is often used to ensure proper encapsulation or to maintain consistency in behavior

Example:
```cpp
class Base {
public:
    void debug() { cout << "Base debug" << endl; }
};

class Derived : public Base {
public:
    void debug() = delete; // Prevent accidental use of debug in derived class
};

int main() {
    Derived d;
    // d.debug(); // Compilation error: debug is deleted in Derived
    return 0;
}
```

## Why Use Function Hiding?

### 1. Control Interface Exposure:

- Derived classes can provide a more tailored interface by hiding irrelevant or potentially confusing base class methods.

### 2. Prevent Misuse:

- By hiding base class functions, you ensure that derived class users cannot invoke base class functionality that is inappropriate or undesired.
Simplify Derived Class Functionality:

- Hiding unused or irrelevant base class overloads can make the derived class simpler to use.

### 3. Override Behavior:

- Function hiding can effectively allow derived classes to provide their implementation while suppressing access to base class implementations.

## Considerations

### 1. Clarity:

- Ensure function hiding is intentional and documented; otherwise, it can lead to confusion for developers unfamiliar with the code.

### 2. Avoid Accidental Hiding:

Use using statements to bring hidden base class functions back into scope if needed:

```cpp
using Base::functionName;
```

### 3. Alternatives:

- If function hiding is not strictly needed, consider overriding the function using polymorphism to ensure a consistent interface and behavior.

### Conclusion
- Function hiding is useful when you need precise control over which functions are exposed in a derived class. However, it should be used cautiously and intentionally, with clear reasoning and documentation, to avoid unexpected behavior or maintenance issues.





__________________________________________________________________________________________________________________________________________________________________________________________________


 ## More Info
 
 As we learned earlier that we can have functions with the same name but differerent parameters through function overloading.

We already know that we can have functions with the same method name and same parameters in different classes if the classes are independent of each other. Go through the below code and run it to see what happens.

```
#include <bits/stdc++.h>
using namespace std;

class MobilePhone {
public:
	void print() {
		cout << "MobilePhone" << endl;
	}
};

class Laptop {
public:
	void print() {
		cout << "Laptop" << endl;
	}
};

int main() {
	MobilePhone mobilePhone;
	Laptop laptop;
	mobilePhone.print(); //This will print "MobilePhone"
	laptop.print(); //This will print "Laptop"
	return 0;
}
```

ince these classes are independent of each other, one class does not care about which methods are there in the other class.

Now, let's say there are 2 classes and both of them are related through inheritance. Go through the below code and run it to see what happens.

```
#include <bits/stdc++.h>
using namespace std;

class BaseClass {
public:
	void print() {
		cout << "Base Class" << endl;
	}
};

class DerivedClass: BaseClass {
public:
	void print() {
		cout << "Derived Class" << endl;
	}
};

int main() {
	BaseClass base;
	DerivedClass derived;
	base.print(); //This will print "Base Class"
	derived.print(); //This will print "Derived Class"
	return 0;
}
```

Here, we are seeing the same behavior as the case when there was no relationship. The function of the respective class is called. But there is a different reason for it.

Go through the below code properly and read on. Do not run it yet.

```
#include <bits/stdc++.h>
using namespace std;

class BaseClass {
public:
	void print() {
		cout << "Base Class" << endl;
	}

	void print(int num) {
		cout << "Base Class " << num << endl;
	}

	void print(string name) {
		cout << "Base Class " << name << endl;
	}
};

class DerivedClass:BaseClass {
public:
	void print() {
		cout << "Derived Class" << endl;
	}
};

int main() {
	BaseClass base;
	DerivedClass derived;
	base.print(); //This will print "Base Class"
	base.print(42); //This will print "Base Class 42"
	base.print("workat.tech"); //This will print "Base Class workat.tech"
	
	derived.print(); //This will print "Derived Class"
	derived.print(42); //This should ideally print "Base Class 42"
	derived.print("workat.tech"); //This should ideally print "Base Class workat.tech"
	return 0;
}
```
Here we have created a function named print:

- in the derived class
- with the same name as multiple functions in the base class
Ideally, all the public members (properties and functions) of the base class should be available through the derived class as well (because of inheritance).

So the expected behavior here is:

- base.print(); //This will print "Base Class"
- base.print(42); //This will print "Base Class 42"
- base.print("workat.tech"); //This will print "Base Class workat.tech"
- derived.print(); //This will print "Derived Class"
- derived.print(42); //This should ideally print "Base Class 42"
- derived.print("workat.tech"); //This should ideally print "Base Class workat.tech"

Now run the above code.

After running it, we can see that we get a compilation error because:

- any function named print from the base class
- won't be available through the derived class
This is not the expected behavior, right?

This is known as function hiding. Whenever a function with the same name is present in both base class and derived class, the functions with that name in the base class won't be available through the derived class irrespective of the parameters.

This is the reason why "Base Class" was getting printed on doing base.print() and "Derived Class" on doing derived.print() in this code:

```
BaseClass base;
DerivedClass derived;
base.print(); //This will print "Base Class"
derived.print(); //This will print "Derived Class"
```
