**Class Templates**

Let's create an Array class which contains an integer array and expose a few methods to work with the array. Go through the below code properly.

```
#include <bits/stdc++.h>
using namespace std;
class ArrayClass {
private:
	int* arr;
	int current_size = 0;
	int max_size;
public:
	ArrayClass(int max_size) {
		this->max_size = max_size;
		arr = new int[max_size];
	}
	
	void insert(int element) {
		arr[current_size] = element;
		current_size++;
	}

	int get_at(int index) {
		return arr[index];
	}
	
	void print() {
		for (int iterator = 0; iterator < current_size; iterator++) {
			cout << arr[iterator] << " ";
		}
		cout << endl;
	}
};

int main() {
	// your code goes here
	ArrayClass array(10);
	array.insert(5);
	array.insert(6);
	array.insert(7);
	array.insert(8);
	array.print();
	cout << array.get_at(2);
	return 0;
}

```
Let's look at what is happening here:

-The Array class takes the max_size in the constructor and creates an array with that size.
-The insert method takes an element and adds it at the last index (current_size) and increments the current_size.
-The get_at methods takes an index and returns the element at that index.
-The print method prints the array.

Now, we have a class for int arrays. What if we want to create similar classes for arrays of other data types as well?

Similar to function templates, we can also have class templates. Just like function templates, class templates allow us to create template data types and use them 
inside the class to make the class generic.

Let's templatize the above code to create array of any data type (could be int, float as well as Employee or any other class).

Here, we have just replaced int with T wherever int was mentioned in the context of array element.

```
template<class T>
class ArrayClass {
private:
	T* arr;
	int current_size = 0;
	int max_size;
public:
	ArrayClass(int max_size) {
		this->max_size = max_size;
		arr = new T[max_size];
	}
	
	void insert(T element) {
		arr[current_size] = element;
		current_size++;
	}

	T get_at(int index) {
		return arr[index];
	}
	
	void print() {
		for (int iterator = 0; iterator < current_size; iterator++) {
			cout << arr[iterator] << " ";
		}
		cout << endl;
	}
};
```

Everything looks fine, right?

There is one major issue. When we create the array object like this:

```
ArrayClass array(10);
```
The class won't be able to implicitly find out what is the data type of T. It could be int or float or anything. It will be unknown until a function call is made
where T is present in parameters (In this case, insert method).

So, how do we fix it?

We have already learned in the previous section how to explicitly mention the data type while calling a template function like this:

max_num<int>(firstInt, secondInt)
The same thing can be done here as well like this:

-ArrayClass<int> array(10);
-ArrayClass<float> array2(10);
-ArrayClass<Employee> array3(10);
Class Templates are very commonly used in C++ Standard Template Library (STL).

The UML diagram for the above class would be:
  
  ----------------------------|
  |      ArrayClass           |
  ----------------------------|
  | arr:T[]                   |
  | current_size: int         |
  | max_size:int              |
  ----------------------------|
  | ArrayClass(max_size:int)  |
  | insert(element: T):void   |
  | get_at(index:int):T       |
  | print():void              |
  ----------------------------|
  UML Diagram gor ArrayClass
  
  
  Let's modify the below code to use templates

-Templatize the Stack class in the editor to allow the container array to work for non-int elements as well.
-Modify the object creation in the main method to work with the templatized Stack class.
-I've added comments wherever you need to templatize. Modify only those parts.
-Just focus on the templatizing part and the object creation part.
-You can ignore the logic for now.

  *Expected Output*
  
  ```
0 1 0
5 0 1
0 1 0
0 1 0
3.14 0 1
0 1 0
 1 0
w 0 1
 1 0
```

```
#include <bits/stdc++.h>
using namespace std;

//Templatize the class
template<class T>
class Stack {
private:
	T *container; //Array for storing data. Needs to be templatized
	int no_of_elements = 0;
	int max_size;
public:
	Stack (int max_size) {
		this->max_size = max_size;
	 	//Array initialization. Needs to be templatized
		container = new T[max_size];
	}

	bool empty() {
		return no_of_elements == 0;
	}
	
	int size() {
		return no_of_elements;
	}
	
	//Returns array element. Right now return_type is int. Needs to be templatized
	T top() {
		if (no_of_elements == 0) {
			return NULL;
		}
		return container[no_of_elements - 1];
	}
	
	//Takes element to be added to array. Right now param_type is int. Needs to be templatized
	void push(T element) {
		if (no_of_elements == max_size) {
			return;
		}
		container[no_of_elements] = element;
		no_of_elements++;
	}
	
	void pop() {
		if (no_of_elements == 0) {
			return;
		}
		no_of_elements--;
	}
};

int main() {
	// your code goes here
	//Templatize this for int
	Stack<int> stackInt(100);
	cout << stackInt.top() << " " << stackInt.empty() << " " << stackInt.size() << endl;
	stackInt.push(5);
	cout << stackInt.top() << " " << stackInt.empty() << " " << stackInt.size() << endl;
	stackInt.pop();
	cout << stackInt.top() << " " << stackInt.empty() << " " << stackInt.size() << endl;
	
	//Templatize this for float
	Stack<float> stackFloat(100);
	cout << stackFloat.top() << " " << stackFloat.empty() << " " << stackFloat.size() << endl;
	stackFloat.push(3.14);
	cout << stackFloat.top() << " " << stackFloat.empty() << " " << stackFloat.size() << endl;
	stackFloat.pop();
	cout << stackFloat.top() << " " << stackFloat.empty() << " " << stackFloat.size() << endl;
	
	//Templatize this for char
	Stack<char> stackChar(100);
	cout << stackChar.top() << " " << stackChar.empty() << " " << stackChar.size() << endl;
	stackChar.push('w');
	cout << stackChar.top() << " " << stackChar.empty() << " " << stackChar.size() << endl;
	stackChar.pop();
	cout << stackChar.top() << " " << stackChar.empty() << " " << stackChar.size() << endl;
	
	
	return 0;
}
 ```
  
  
**Templates**
Both function templates and class templates allow us to write generic code as in functions and class operating on generic types. Generic programming is a style of 
  computer programming in which code is written in terms of types to-be-specified-later as we have been doing in this chapter by adding a template data-type/class.

The C++ Standard Library includes the Standard Template Library or STL that provides a framework of templates for common data structures and algorithms.

This type of polymorphism is also known as parametric polymorphism.

Just like in function overloading and operator overloading, the actual function/class to use is determined at compile time in case of templates as well and so 
templates is also a way to achieve compile-time polymorphism.
  
Problem Statement:
Given a class Pair which is used to store data in pairs.
 
------------------------------------|
|            *PairClass*            |
|-----------------------------------|
| - first:T                         |
| - second:U                        |
| ----------------------------------|
| + PairClass(T first, U second)    |
| + getFirst(): T                   |
| + getSecond():U                   |
| + setFirst(T first):void          |
| + void setSecond(T second):void   |
-------------------------------------
  UML Diagram for PairClass Class
  
  First and second can be of any data type irrespective of each other.

*Examples*
  
 ```
firstPair => (4, 2)
secondPair => (3.14, 2.718)
thirdPair => ("Hello", 3.14)
```

Implementation Details:

- All get and set functions are normal getters and setters.
- `getFirst` returns `first`
- `getSecond` returns `second`
- `setFirst` takes first as argument and assigns it to the attribute `first`.
- `setSecond` takes second as argument and assigns it to the attribute `second`.
- In the main method, create three pairs with the following values:
- `firstPair` => (4, 2)
- `secondPair` => (3.14, 2.718)
- `thirdPair` => ("Hello", 3.14)
Do not modify anything in the code after the "DO NOT MODIFY AFTER THIS" comment.

 *Expected Output*
  
```
4 2
3.14 2.718
Hello 3.14
3 14
```

```
  #include <bits/stdc++.h>
using namespace std;

template<class T, class U>

class PairClass{
	
	 T first;
	 U second;
	 
	public:
	PairClass(T first, U second){
		this->first=first;
		this->second=second;
	}
	
	T getFirst(){
		return this->first;
	}
	
	U getSecond(){
		return this->second;
	}
	
	void setFirst(T first){
		this->first=first;
	}
	
	void setSecond(T second){
		 this->second=second;
	}
	
	
};

int main() {
	// your code goes here
	
	PairClass<int,int> firstPair(4,2);
	PairClass<float, float> secondPair(3.14, 2.718);
	PairClass<string, float> thirdPair("Hello", 3.14);
	
	
	//DO NOT MODIFY AFTER THIS
	cout << firstPair.getFirst() << " " << firstPair.getSecond() << endl;
	cout << secondPair.getFirst() << " " << secondPair.getSecond() << endl;
	cout << thirdPair.getFirst() << " " << thirdPair.getSecond() << endl;
	
	firstPair.setFirst(3);
	firstPair.setSecond(14);
	cout << firstPair.getFirst() << " " << firstPair.getSecond() << endl;
	
	return 0;
}
 
 ```
