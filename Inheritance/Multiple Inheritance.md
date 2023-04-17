In the Hierarchy Inheritence, we have seen the following relationship:
Every CameraPhone is-a Phone

While this is true, it should be noted that the following relationship is also true:
Every CameraPhone is-a Camera

Therefore, we can say that:
Every CameraPhone is-a Phone as well is-a Camera

This type of relationship where a class inherits multiple base classes is known as multiple inheritance. It can be represented as:

![image](https://user-images.githubusercontent.com/45598340/232575859-8740d434-4e5c-4acc-a264-f37c3f718863.png)

The code would look something like this:

```
class Phone {
    .
    .
    .
};

class Camera {
    .
    .
    .
};

class CameraPhone: public Phone, public Camera {
public:
	CameraPhone(string brand, string model, int ram, int storage, double resolution): Phone(brand, model, ram, storage), Camera(resolution) {
	}
};
```

Here we mention the base classes as comma-separated-values with the inheritance-access-specifier. The constructors are also mentioned comma-separated with the required parameters.

In this particular example, CameraPhone does not have any property of its own and so we've not mentioned anything inside the constructor method. If there would have been a property in CameraPhone which was not inherited from its base classes, we can initialize it in the constructor just like we have been doing previously.

Let's write the code for the UML diagram.

![image](https://user-images.githubusercontent.com/45598340/232576148-1d6823ba-8d03-4a08-b8d4-0f28a1caeb2d.png)

Use the function definitions from Inheritance:Introduction section.

`clickPhoto` should print:

```
Clicking photo on <resolution_value> MP
```

**Expected Output**
```
Apple iPhone 4 64
Calling 9732130450 from Apple:iPhone
Receiving call from 9732130450 on Apple:iPhone
24.1
Clicking photo on 24.1 MP
Apple iPhone 8 4 64 12
Calling 9732130450 from Apple:iPhone 8
Receiving call from 9732130450 on Apple:iPhone 8
Clicking photo on 12 MP
```

```
#include <bits/stdc++.h>
using namespace std;

class Phone{
	string brand;
	string model;
	int ram;
	int storage;
	
	public:
	Phone(string brand, string model, int ram, int storage):brand{brand},
	model{model},ram{ram},storage{storage}{}
	
	string getBrand(){
		return brand;
	}
	
	string getModel(){
		return model;
	}
	
	int getRam(){
		return ram;
	}
	
	int getStorage(){
		return storage;
	}
	
	void dialCall(string number){
		cout<<"Calling "<<number <<" from "<<getBrand()<<":"<<getModel()<<endl;
	}
	void receiveCall(string number){
	  cout<<"Receiving call from "<<number<<" on "<<getBrand()<<":"<<getModel()<<endl;
	}
	
};
class Camera{
	double resolution;
	
	public:
	Camera(double resolution):resolution{resolution}{}
	
	double getResolution(){
		return resolution;
	}
	void clickPhoto(){
		cout<<"Clicking photo on "<<getResolution()<<" MP"<<endl;
	}
};

class CameraPhone:public Phone, public Camera{
	
	public:
	CameraPhone(string brand, string model, int ram, int storage, double resolution):Phone(brand, model,ram,storage), Camera(resolution){}
};
int main() {
	// do not modify the main method
	Phone phone("Apple", "iPhone", 4, 64);
	Camera camera(24.1);
	CameraPhone cameraPhone("Apple", "iPhone 8", 4, 64, 12);
	
	cout << phone.getBrand() << " " << phone.getModel() << " " << phone.getRam() << " " << phone.getStorage() << endl;
	phone.dialCall("9732130450");
	phone.receiveCall("9732130450");
	
	cout << camera.getResolution() << endl;
	camera.clickPhoto();
	
	cout << cameraPhone.getBrand() << " " << cameraPhone.getModel() << " " << cameraPhone.getRam() << " " << cameraPhone.getStorage() << " " << cameraPhone.getResolution() << endl;
	cameraPhone.dialCall("9732130450");
	cameraPhone.receiveCall("9732130450");
	cameraPhone.clickPhoto();
	return 0;
}
```



