Apart from single inheritance where we inherit a derived class from a base class, we have learnt that we can:

- Inherit multiple derived classes from a base class
- Inherit a derived class from multiple base classes
- Inherit a derived class from a base class which is derived from some other base class

It seems quite obvious that we can have classes where we have more than one of the above. In fact, we have seen an example of the same previously.

- Every Phone `is-a` ElectronicDevice
- Every Camera `is-a` ElectronicDevice
- Every CameraPhone `is-a` Phone
- Every CameraPhone `is-a` Camera
Here, we have all of the three types of inheritance discussed above:

- Hierarchical Inheritance: Phone, Camera -> ElectronicDevice
- Multiple Inheritance: CameraPhone -> Phone, Camera
- Multilevel Inheritance: CameraPhone -> Phone -> ElectronicDevice
Such types of inheritance is known as Hybrid Inheritance (Combination of more than 1 type of inheritance).

UML Diagram of the above example:

![image](https://user-images.githubusercontent.com/45598340/232578713-37d0c7b3-482a-4282-9895-51f5b55d1fcf.png)

The code would look something like this:

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

class CameraPhone: public Phone, public Camera {
public:
	CameraPhone(string brand, string model, int ram, int storage, double resolution): Phone(brand, model, ram, storage), Camera(brand, model, resolution) {
};

```



