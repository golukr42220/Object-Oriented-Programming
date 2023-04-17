**Hierarchical Inheritance**

In Single Inheritance, we had one class inheriting from another class. Similarly, we have Hierarchical Inheritance in which multiple classes inherit a single base class.

![image](https://user-images.githubusercontent.com/45598340/232574986-c069f7d2-801e-4cc8-84bd-4d1623a717b5.png)

There can be any number of derived classes and each derived class can have properties different from one another. The code for this will be exactly the same as Single Inheritance.

```
class ElectronicDevice {
    .
    .
    .
};

class Phone: public ElectronicDevice {
    .
    .
    .
};

class Camera: public ElectronicDevice {
    .
    .
    .
};
```

Let's write some code.

- Create classes based on the UML diagram.

![image](https://user-images.githubusercontent.com/45598340/232575227-5ea34416-8531-48bb-97a0-2befbd0fc2cb.png)

- Do not modify the main method.
Use the function definitions from Inheritance:Introduction section.

**Expected Output**
```
Apple iPhone 4 64
Calling 9732130450 from Apple:iPhone
Receiving call from 9732130450 on Apple:iPhone
Canon EOS 1500D 24.1
Clicking photo on a 24.1 MP Canon:EOS 1500D
```

```
#include <bits/stdc++.h>
using namespace std;
class ElectronicDevice{
	
	
	string brand;
	string model;
	public:
	
	ElectronicDevice(string brand, string model):brand{brand}, model{model}{}
	
	string getBrand(){
		return brand;
	}
	string getModel(){
		return model;
	}

	
};

class Phone :public ElectronicDevice{
	
	int ram;
	int storage;
	public:
	
	Phone(string brand, string model, int ram, int storage):ElectronicDevice(brand, model),ram{ram}, storage{storage}{}
	
	int getRam(){
		return ram;
	}
	
	int getStorage(){
		return storage;
	}
	void dialCall(string number){
		cout<<"Calling "<<number<<" from "<<this->getBrand()<<":"<<this->getModel()<<endl;
	}
	
	void receiveCall(string number){
		cout<<"Receiving call from "<<number<<" on "<<this->getBrand()<<":"<<this->getModel()<<endl;
	}
	
};

class Camera : public ElectronicDevice{
	
	double resolution;
	public:
	Camera(string brand ,string model ,double resolution):ElectronicDevice(brand, model),resolution{resolution}{}
	
	double getResolution(){
		return resolution;
	}
	void clickPhoto(){
		cout<<"Clicking photo on a "<< this->getResolution()<<" MP "<<this->getBrand()<<":"<<this->getModel()<<endl;
	}
};
int main() {
	// do not modify the main method
	Phone phone("Apple", "iPhone", 4, 64);
	Camera camera("Canon", "EOS 1500D", 24.1);
	
	cout << phone.getBrand() << " " << phone.getModel() << " " << phone.getRam() << " " << phone.getStorage() << endl;
	phone.dialCall("9732130450");
	phone.receiveCall("9732130450");
	
	cout << camera.getBrand() << " " << camera.getModel() << " " << camera.getResolution() << endl;
	camera.clickPhoto();
	return 0;
}
```
