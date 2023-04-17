
In Single Inheritance, there is a derived class which extends a base class whereas the base class is standalone and does not extend anything.

Similar to Single Inheritance, we can have Multi-level Inheritance as well where the base class extends some other base class.

**Example**
- Every Phone is-a ElectronicDevice
- Every CameraPhone is-a Phone
Assuming that brand and model are properties of ElectronicDevice.

This can be represented as:

Here Phone extends ElectronicDevice and CameraPhone extends Phone.

The code would look something like this:
![image](https://user-images.githubusercontent.com/45598340/232577739-1283db26-cde7-421d-8b61-3f8a344c0106.png)

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

class CameraPhone: public Phone {
    .
    .
    .
};
```

Taking the Car-Vehicle example, SelfDrivingCar can be one class that extends Car.

Let's write some code.

- Create classes based on the UML diagram.

![image](https://user-images.githubusercontent.com/45598340/232578064-06dc584d-5103-4c8e-ba67-a14d4d1e1d5f.png)

- Do not modify the main method.
Use the function definitions from previous sections.

**Expected Output**
```
Fitbit Versa 3
Apple iPhone 4 64
Calling 9732130450 from Apple:iPhone
Receiving call from 9732130450 on Apple:iPhone
Apple iPhone 8 4 64 12
Calling 9732130450 from Apple:iPhone 8
Receiving call from 9732130450 on Apple:iPhone 8
Clicking photo on a 12 MP Apple:iPhone 8
```

```
#include <bits/stdc++.h>
using namespace std;

class ElectronicDevice{
	string brand;
	string model;
	
	public:
	
	ElectronicDevice(string brand, string model):brand{brand},model{model}{}
	
	string getBrand(){ return brand;}
	string getModel(){return model;}
   	
};

class Phone: public ElectronicDevice{
	int ram;
	int storage;
	
	public:
	Phone(string brand, string model, int ram, int storage):ElectronicDevice(brand, model), ram{ram},storage{storage}{}
	
	int getRam(){ return ram;}
	int getStorage(){return storage;}
	void dialCall(string number){
		cout<<"Calling "<<number<< " from "<<this->getBrand()<<":"<<this->getModel()<<endl;
	}
	
	void receiveCall(string number){
		cout <<"Receiving call from "<<number<<" on "<<this->getBrand()<<":"<<this->getModel()<<endl;
	}
};

class CameraPhone: public Phone{
	int resolution;
	
	public:
	CameraPhone(string brand, string model, int ram, int storage, int resolution ):Phone(brand, model,ram, storage),resolution{resolution}{}
	
	int getResolution(){return  resolution;}
	
	void clickPhoto(){
		cout<<"Clicking photo on a "<<getResolution()<<" MP "<<this->getBrand()<<":"<<this->getModel()<<endl;
	}
};
int main() {
	// do not modify the main method
	ElectronicDevice electronicDevice("Fitbit", "Versa 3");
	Phone phone("Apple", "iPhone", 4, 64);
	CameraPhone cameraPhone("Apple", "iPhone 8", 4, 64, 12);
	
	cout << electronicDevice.getBrand() << " " << electronicDevice.getModel() << endl;

	cout << phone.getBrand() << " " << phone.getModel() << " " << phone.getRam() << " " << phone.getStorage() << endl;
	phone.dialCall("9732130450");
	phone.receiveCall("9732130450");
	
	cout << cameraPhone.getBrand() << " " << cameraPhone.getModel() << " " << cameraPhone.getRam() << " " << cameraPhone.getStorage() << " " << cameraPhone.getResolution() << endl;
	cameraPhone.dialCall("9732130450");
	cameraPhone.receiveCall("9732130450");
	cameraPhone.clickPhoto();
	return 0;
}
```

