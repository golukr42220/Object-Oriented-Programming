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
