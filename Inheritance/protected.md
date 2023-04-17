As we have learnt previously that a derived class cannot access the private members of the base class. What if we want to use those members in the derived class?

One solution is to make that class member as public instead of private.

But if we do that, we will be exposing that property for any other class to access and compromising on encapsulation.

To resolve this, C++ provides us with another access specifier: protected.

**Example**
```
class ElectronicDevice {
protected:
	string brand;
	string model;
public:
	ElectronicDevice (string brand, string model) {
		this->brand = brand;
		this->model = model;
	}
	.
	.
	.
	.
};
```

When a class has a particular class member set as protected, the member is accessible to that particular class and any other class which inherits that base class.

Now, in the following relationship, ram and storage will be accessible in CameraPhone but not in the main method. Here, # represents protected.

![image](https://user-images.githubusercontent.com/45598340/232579204-a87dd5af-2908-46a4-a414-b3b8b1591dce.png)


Let's modify the below code to use protected based on the above UML diagram.

Use the function definitions from previous sections.

**Expected Output**

```
Fitbit Versa 3
Apple iPhone 4 64
Calling 9732130450 from Apple:iPhone
Receiving call from 9732130450 on Apple:iPhone
Canon EOS 1500D 24.1
Clicking photo on a 24.1 MP Canon:EOS 1500D
Apple iPhone 8 4 64 12
Calling 9732130450 from Apple:iPhone 8
Receiving call from 9732130450 on Apple:iPhone 8
Clicking photo on a 12 MP Apple:iPhone 8
```

```
#include <bits/stdc++.h>
using namespace std;

class ElectronicDevice {
protected:
	string brand;
	string model;
public:
	ElectronicDevice (string brand, string model) 
	{
		this->brand = brand;
		this->model = model;
	}
	string getBrand()
	{
		return brand;
	}
	string getModel()
	{
		return model;
	}
};
class Phone: public ElectronicDevice
{
protected:
	int ram;
	int storage;
public:
	Phone(string brand,string model,int ram,int storage): ElectronicDevice(brand,model)
	{
		 this->ram=ram;
		 this->storage=storage;
	}
	int getRam()
	{
		return ram;
	}
	int getStorage()
	{
		return storage;
	}
	void dialCall(string number)
	{
		cout << "Calling " << number << " from " << brand << ":" << model << endl;
	}
	
	void receiveCall(string number)
	{
		cout << "Receiving call from " << number << " on " << brand << ":" << model << endl;
	}
};
class Camera: public ElectronicDevice
{
private:
	double resolution;
public:
	Camera(string brand,string model,double resolution): ElectronicDevice(brand,model)
	{
		this->resolution=resolution;
	}
	double getResolution()
	{
		return resolution;
	}
	void clickPhoto()
	{
		cout<<"Clicking photo on a "<<resolution<<" MP "<<brand<<":"<< model << endl;
	}
	
};
class CameraPhone: public Phone, public Camera 
{
public:
	CameraPhone(string brand, string model, int ram, int storage, double resolution): Phone(brand, model, ram, storage), Camera(brand, model, resolution) {
	}
};
	
int main() {
	// do not modify the main method
	ElectronicDevice electronicDevice("Fitbit", "Versa 3");
	Phone phone("Apple", "iPhone", 4, 64);
	Camera camera("Canon", "EOS 1500D", 24.1);
	CameraPhone cameraPhone("Apple", "iPhone 8", 4, 64, 12);
	
	cout << electronicDevice.getBrand() << " " << electronicDevice.getModel() << endl;

	cout << phone.getBrand() << " " << phone.getModel() << " " << phone.getRam() << " " << phone.getStorage() << endl;
	phone.dialCall("9732130450");
	phone.receiveCall("9732130450");
	
	cout << camera.getBrand() << " " << camera.getModel() << " " << camera.getResolution() << endl;
	camera.clickPhoto();
	
	cout << cameraPhone.Phone::getBrand() << " " << cameraPhone.Phone::getModel() << " " << cameraPhone.getRam() << " " << cameraPhone.getStorage() << " " << cameraPhone.getResolution() << endl;
	cameraPhone.dialCall("9732130450");
	cameraPhone.receiveCall("9732130450");
	cameraPhone.clickPhoto();
	return 0;
}
```



