In the previous sections, we learned that:

- We can access class members through an object using the dot operator.
- We can use pointers to access class elements by using the arrow operator.
- We can create pointers of base class and point it to an object of derived class.
- We can use these pointers to access class members of derived class that are inherited from the base class.
- We cannot use these pointers to access class members of derived class that are not inherited from the base class.
- Calling a method present in both base class and derived class through a pointer of base class will result in the method from base class being called.

Now, our problem here is that if we want to abstract out the actual class with the base class, we lose access to methods that we might want to use. Given a list of phones, we would want the display method to print the value based on the display method in the respective class.

To solve this problem, what we would want to do is override the function in the base class with the function in the derived class. For a particular function in the base class, if there is no other implementation present in the derived class then there is no problem as the base function is the one that needs to be called anyway. What we need here is the flexibility to add a different function implementation in any of the derived classes and use that function if it has been overridden in the derived class. This is known as function overriding.

Now we know what is the problem and what we want to do. Let's see how to do it.

C++ has a way to override a function through the virtual keyword.

Based on the use case if we want a base class function to be overridden we can add the virtual keyword and then any derived class would be able to override it. Function overriding with the virtual keyword is used quite often when we are writing code using OOP in C++.

**Syntax**

```
class ClassName {
	.
	.
	.
	virtual return_type function_name(function_parameter_1, function_parameter_2, …..) {
		//function logic
	}
}
```

**Example**

```
class Phone {
	.
	.
	.
	virtual void display () {
		cout << getBrand() << " " << getModel() <<  " " << getRam() <<  " " << getStorage() << endl;
	}
};

class CameraPhone {
	.
	.
	.
	void display () {
		cout << getBrand() << " " << getModel() <<  " " << getRam() <<  " " << getStorage() << " " << getResolution() << endl;
	}
}
```

Note that I have added the virtual keyword in the display method of class Phone.

Now, we will get the desired output when we do this:

```
Phone phone("Nokia", "110", 1, 1);
CameraPhone camera_phone("Apple", "iPhone", 4, 64, 12);
Phone *phone_ptr_1 = &phone;
Phone *phone_ptr_2 = &camera_phone;
phone_ptr_1->display();
phone_ptr_2->display();
```

**Expected Output**
```
Nokia 110 1 1
Apple iPhone 4 64 12
```
Let's see it in action. Complete the tasks mentioned in the comments and then hit run to check if we are getting the expected output (mentioned above).

```
#include <bits/stdc++.h>
using namespace std;

class Phone {
	string brand;
	string model;
	int ram;
	int storage;
public:
	Phone (string brand, string model, int ram, int storage) {
		this->brand = brand;
		this->model = model;
		this->ram = ram;
		this->storage = storage;
	}

    string getBrand() {
        return this->brand;
    }

    string getModel() {
        return this->model;
    }

    int getRam() {
        return this->ram;
    }

    int getStorage() {
        return this->storage;
    }
    void dialCall (string number) {
    	cout << "Calling " << number << " from " << brand << ":" << model << "\n";
    }
	
    void receiveCall (string number) {
    	cout << "Receiving call from " << number << " on " << brand << ":" << model << "\n";
    }

    // Task: Add the virtual function display with the implementation
	virtual void display(){
		cout << getBrand() << " " << getModel() <<  " " << getRam() <<  " " << getStorage() << endl;
	}
};

class CameraPhone: public Phone {
    double resolution;
public:
    CameraPhone(string brand, string model, int ram, int storage, double resolution): Phone(brand, model, ram, storage) {
    	this->resolution = resolution;
    }

    double getResolution() {
        return this->resolution;
    }
	
    void clickPhoto () {
    	cout << "Clicking photo on a " << resolution << " MP " << getBrand() << ":" << getModel() << "\n";
    }

    // Task: Add the function display with the implementation
	void display () {
		cout << getBrand() << " " << getModel() <<  " " << getRam() <<  " " << getStorage() << " " << getResolution() << endl;
	}
};

int main () {
	Phone phone("Nokia", "110", 1, 1);
	CameraPhone camera_phone("Apple", "iPhone", 4, 64, 12);
	Phone *phone_ptr_1 = &phone;
	Phone *phone_ptr_2 = &camera_phone;
	phone_ptr_1->display();
	phone_ptr_2->display();
	return 0;
}

```

**Virtual Functions**

A virtual function is a member function which is declared within a base class and is re-defined (overridden) by a derived class. When you refer to a derived class object using a pointer or a reference to the base class, you can call a virtual function for that object and execute the derived class’s version of the function. 

- Virtual functions ensure that the correct function is called for an object, regardless of the type of reference (or pointer) used for function call.
- They are mainly used to achieve [Runtime polymorphism]([url](https://www.geeksforgeeks.org/cpp-polymorphism/))
- Functions are declared with a virtual keyword in base class.
- The resolving of function call is done at runtime.

**Rules for Virtual Functions**

1. Virtual functions cannot be static.
2. A virtual function can be a friend function of another class.
3. Virtual functions should be accessed using pointer or reference of base class type to achieve runtime polymorphism.
4. The prototype of virtual functions should be the same in the base as well as derived class.
5. They are always defined in the base class and overridden in a derived class. It is not mandatory for the derived class to override (or re-define the virtual function), in that case, the base class version of the function is used.
6. A class may have [virtual destructor]([url](https://www.geeksforgeeks.org/virtual-destructor/)) but it cannot have a virtual constructor.

**Compile time (early binding) VS runtime (late binding) behavior of Virtual Functions**
Consider the following simple program showing runtime behavior of virtual functions.

```
// CPP program to illustrate
// concept of Virtual Functions

#include<iostream>
using namespace std;

class base {
public:
	virtual void print()
	{
		cout << "print base class\n";
	}

	void show()
	{
		cout << "show base class\n";
	}
};

class derived : public base {
public:
	void print()
	{
		cout << "print derived class\n";
	}

	void show()
	{
		cout << "show derived class\n";
	}
};

int main()
{
	base *bptr;
	derived d;
	bptr = &d;

	// Virtual function, binded at runtime
	bptr->print();

	// Non-virtual function, binded at compile time
	bptr->show();
	
	return 0;
}
```
**Output:**  
```
print derived class
show base class
```
**Explanation:** Runtime polymorphism is achieved only through a pointer (or reference) of base class type. Also, a base class pointer can point to the objects of base class as well as to the objects of derived class. In above code, base class pointer ‘bptr’ contains the address of object ‘d’ of derived class.
Late binding (Runtime) is done in accordance with the content of pointer (i.e. location pointed to by pointer) and Early binding (Compile time) is done according to the type of pointer, since print() function is declared with virtual keyword so it will be bound at runtime (output is print derived class as pointer is pointing to object of derived class) and show() is non-virtual so it will be bound during compile time (output is show base class as pointer is of base type).

**NOTE:** If we have created a virtual function in the base class and it is being overridden in the derived class then we don’t need virtual keyword in the derived class, functions are automatically considered as virtual functions in the derived class.

**Working of virtual functions (concept of VTABLE and VPTR)**

As discussed [here]([url](https://www.geeksforgeeks.org/virtual-functions-and-runtime-polymorphism-in-cpp/)), if a class contains a virtual function then compiler itself does two things.

**1.** If object of that class is created then a virtual pointer (VPTR) is inserted as a data member of the class to point to VTABLE of that class. For each new object created, a new virtual pointer is inserted as a data member of that class.
**2.** Irrespective of object is created or not, class contains as a member a static array of function pointers called VTABLE. Cells of this table store the address of each virtual function contained in that class.
Consider the example below: 

![image](https://user-images.githubusercontent.com/45598340/232892084-58d6f624-061e-4469-b5a7-598105feba25.png)

```
// CPP program to illustrate
// working of Virtual Functions
#include<iostream>
using namespace std;

class base {
public:
	void fun_1() { cout << "base-1\n"; }
	virtual void fun_2() { cout << "base-2\n"; }
	virtual void fun_3() { cout << "base-3\n"; }
	virtual void fun_4() { cout << "base-4\n"; }
};

class derived : public base {
public:
	void fun_1() { cout << "derived-1\n"; }
	void fun_2() { cout << "derived-2\n"; }
	void fun_4(int x) { cout << "derived-4\n"; }
};

int main()
{
	base *p;
	derived obj1;
	p = &obj1;

	// Early binding because fun1() is non-virtual
	// in base
	p->fun_1();

	// Late binding (RTP)
	p->fun_2();

	// Late binding (RTP)
	p->fun_3();

	// Late binding (RTP)
	p->fun_4();

	// Early binding but this function call is
	// illegal (produces error) because pointer
	// is of base type and function is of
	// derived class
	// p->fun_4(5);
	
	return 0;
}

```

**Output:**  
```
base-1
derived-2
base-3
base-4
```

**Explanation:** Initially, we create a pointer of type base class and initialize it with the address of the derived class object. When we create an object of the derived class, the compiler creates a pointer as a data member of the class containing the address of `VTABLE` of the derived class.

Similar concept of **Late and Early Binding** is used as in above example. For `fun_1()` function call, base class version of function is called, `fun_2()` is overridden in derived class so derived class version is called, `fun_3()` is not overridden in derived class and is virtual function so base class version is called, similarly `fun_4()` is not overridden so base class version is called.

NOTE: `fun_4(int)` in derived class is different from virtual function `fun_4()` in base class as prototypes of both the functions are different.

**Limitations of Virtual Functions:**
- **Slower:** The function call takes slightly longer due to the virtual mechanism and makes it more difficult for the compiler to optimize because it does not know exactly which function is going to be called at compile time.
- **Difficult to Debug:** In a complex system, virtual functions can make it a little more difficult to figure out where a function is being called from.

