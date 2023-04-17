
In Single Inheritance, there is a derived class which extends a base class whereas the base class is standalone and does not extend anything.

Similar to Single Inheritance, we can have Multi-level Inheritance as well where the base class extends some other base class.

**Example**
- Every Phone is-a ElectronicDevice
- Every CameraPhone is-a Phone
Assuming that brand and model are properties of ElectronicDevice.

This can be represented as:

Here Phone extends ElectronicDevice and CameraPhone extends Phone.

The code would look something like this:

