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
