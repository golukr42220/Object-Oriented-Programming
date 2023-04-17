
*Which of these are class members?*

A. Attributes and Functions

B. Only Attributes

C. Only Functions

D. None of the above

*ANS - A*

There are multiple classes that a phone can belong to. We can define multiple classes based on an hierarchy. Let's look at a few dependencies based on hierarchy.

- Every Human `is-a` Mammal
- Every Mammal `is-a` Organism
- Every Mammal `is not a` Human
- Every Organism `is not a` Mammal
- Every Phone `is-a` ElectronicDevice
- Every CameraPhone `is-a` Phone
- Every ElectronicDevice `is not a` Phone
- Every Phone `is not a` CameraPhone

As we can see that any classification that is below in the hierarchy has an "is-a" relationship with its ancestor.

In this relationship, the ancestor is known as the base class and the descendant is known as the derived class. In case of Phone and CameraPhone (which is-a Phone), Phone is a base class and CameraPhone is the derived class.

If we were to create classes for both, we will see that all the attributes and functions provided by Phone will also be provided by CameraPhone.

**Example**
Phone will have brand, model, ram, storage

CameraPhone will also have brand, model, ram, storage

CameraPhone will have additional attributes and will provide additional functionalities on top of what the Phone class provides.

OOP languages allow us to reuse this and inherit attributes and functions from one class to another. This property is known as Inheritance.

Here, CameraPhone = Phone + extra attributes + extra functions

We can create the Phone and CameraPhone classes like this:

```
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
};
```

Here:

- CameraPhone is derived from Phone class.
- CameraPhone "extends" Phone class.
- Extends relationship => class DerivedClass: public BaseClass
- Here, public is an inheritance-access-specifier. We will learn more about it later. For now, we will keep using public.
- All the "public" class members of the base class is available in the derived class and its objects.
Examples:
- We can call getBrand() inside clickPhoto().
- We can call base class methods on the derived class object like this: cameraPhone.getBrand()
- Private class members of the base class are not available in the derived class.
- The constructor of the base class must be called first in the derived class constructor. We need to pass only the required parameters to the base class constructor.

It is supposed to be done like this:
```
DerivedClass (param1, param2, ...): BaseClass(param1, param2, ….) {
	/** derived class specific initialization **/
}
```
![image](https://user-images.githubusercontent.com/45598340/232573746-7c1cec0d-508b-438d-8bcf-52f95d978cb1.png)

Here, the CameraPhone extends Phone and has class members specific to CameraPhone.

Inheritance UML Representation: An arrow — with a line and a hollow triangle — from the derived class to the base class.

In this section, we learned about Single Inheritance which is a type of Inheritance where one class inherits a single class only.

Let's write some code.

- Create classes based on the below UML diagram.

![image](https://user-images.githubusercontent.com/45598340/232574049-e0e328a7-bddd-472f-9061-6ad14d7552d3.png)

- Print "{brand} is honking" in honk() method.
Example: If brand is Tesla, print "Tesla is honking".
- Print "{brand} {model} is moving" in move() method.
Example: If brand is Tesla and model is "Model S", print "Tesla Model S is moving".
- Do not modify the main method.

**Expected Output**
```
Tesla
Tesla is honking
Tesla Model S
Tesla is honking
Tesla Model S is moving
```

```

#include <bits/stdc++.h>
using namespace std;
class Vehicle{
	string brand;
	public:
	Vehicle(string brand):brand{brand}{}
	string getBrand(){
		return this->brand;
	}
	void honk(){
		cout<< brand <<" is honking"<<endl;
	}
};

class Car: public Vehicle{
	string model;
	public:
	Car(string brand, string model):Vehicle(brand), model{model}{
	}
	
	string getModel(){
		return this->model;
	}
	
	void move(){
		cout<< this->getBrand()<< " "<< this->model<<" is moving"<<endl;
	}
};
int main() {
	// do not modify the main method
	Vehicle vehicle("Tesla");
	Car car("Tesla", "Model S");
	cout << vehicle.getBrand() << endl;
	vehicle.honk();

	cout << car.getBrand() << " " << car.getModel() << endl;
	car.honk();
	car.move();
	return 0;
}
```
