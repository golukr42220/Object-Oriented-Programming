**Function Templates**

We have previously learned that we can have multiple functions with the same name but different parameters in the same file/class through Function Overloading.

```
Example
int max_num (int firstNumber, int secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
long max_num (long firstNumber, long secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
float max_num (float firstNumber, float secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
double max_num (double firstNumber, double secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}

int main() {
	int firstInt = 5, secondInt = 7;
	float firstFloat = 3.14159, secondFloat = 2.71828;
	long firstLong = 54362478632846, secondLong = 7148342783278;
	double firstDouble = 2.71828182845, secondDouble = 3.1415926535;
	
	cout << max_num(firstInt, secondInt) << endl;
	cout << max_num(firstFloat, secondFloat) << endl;
	cout << max_num(firstLong, secondLong) << endl;
	cout << max_num(firstDouble, secondDouble) << endl;

}

```
This is a very valid use case but the issue here is that we have to write a lot of code with the exact same logic. If you notice carefully,
the only difference between these functions is the data types being used. Each of the above functions can be logically written as:

```
data_type max_num (data_type firstNumber, data_type secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}

```
Won't it be great if we can just write like this and the compiler takes care of all the other stuff. Turns out that C++ actually allows us to
achieve this type of polymorphism through something known as TEMPLATES!

Let's see it in action. Run the below code:

```
#include <bits/stdc++.h>
using namespace std;

template <class data_type>
data_type max_num (data_type firstNumber, data_type secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}

int main() {
	int firstInt = 5, secondInt = 7;
	float firstFloat = 3.14159, secondFloat = 2.71828;
	long firstLong = 54362478632846, secondLong = 7148342783278;
	double firstDouble = 2.71828182845, secondDouble = 3.1415926535;
	
	cout << max_num(firstInt, secondInt) << endl;
	cout << max_num(firstFloat, secondFloat) << endl;
	cout << max_num(firstLong, secondLong) << endl;
	cout << max_num(firstDouble, secondDouble) << endl;
	
	return 0;
}
```

As you can see that we have been able to replace the four functions with just a single function by using templates. 
Let's look at how to use templates:

```
template <class identifier>
```
Here,

- template is a keyword to tell the compiler that we are creating a template.
- class is used to tell the compiler that we are creating a datatype or class template.
- identifier is used to tell the compiler what the name of the template would be. This is similar to naming a variable or macro 
and could be almost anything that we want to name it. In the above example it is data_type.
Now, we would have to replace the data types everywhere in the function where we want to use a template data type which we have done 
like this in the above code:

```
data_type max_num (data_type firstNumber, data_type secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
```
In the above code, we have replaced the actual data types with the template type that we created.

Now how this works internally is that during compilation, whenever the compiler comes across any function calls to a template function, 
it creates a new function based on the data types of the arguments.

The newly created function is then used for that particular function call.

In our above example, all the 4 functions that we replaced with the template function will be created by the compiler as all 4 of 
them are supposed to be used in the main method.

Note: the function body needs to be exactly same across multiple functions to replace them with a template function.

Note: For the identifier (template name), a shorter name like T is commonly used which we will be using henceforth. Use T as 
the template name instead of "data_type" (the name that we used in the previous example).

Create a template function in the below code for sum of two numbers
```
Expected Output
12
5.85987
61510821416124
5.85987
```

```
#include <bits/stdc++.h>
using namespace std;

template<class T>
T sum(T firstNumber, T secondNumber) {
	return firstNumber + secondNumber;
}

int main() {
	int firstInt = 5, secondInt = 7;
	float firstFloat = 3.14159, secondFloat = 2.71828;
	long firstLong = 54362478632846, secondLong = 7148342783278;
	double firstDouble = 2.71828182845, secondDouble = 3.1415926535;

	cout << sum(firstInt, secondInt) << endl;
	cout << sum(firstFloat, secondFloat) << endl;
	cout << sum(firstLong, secondLong) << endl;
	cout << sum(firstDouble, secondDouble) << endl;
	
	return 0;
}

```
Now, let's take another case.

In the max_num example, let's say that we had to find the max of firstLong and firstInt like this:

```
cout << max_num(firstLong, firstInt) << endl;
```
Here, the return type is supposed to be long.

Our template function won't work because both the parameters have the same template data type in our template function whereas in the 
function call, we have long and int data as arguments. A function which can take these two as arguments can't be produced from this 
template function.

```
template <class T>
T max_num (T firstNumber, T secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
```
	
What can we do now?

Just like having a template data type T, we can have another data type, say, U. We can rewrite the above code as:

```
template <class T, class U>
T max_num (T firstNumber, U secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
```
	
Here, what is happening is that the first parameter will take data of type T, the second parameter will take data of type U and the function will
return data of type T.

In our example with long and int, the compiler will use this template to create a function like this:
	
```
long max_num (long firstNumber, int secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
```
So now our problem is solved! Will this also work for (int, int), (float, float), (long, long), (double, double)?

The answer is yes. Here, T and U are independent of each other. T can be any data type, U can be any data type independent of each other. 
So it is possible to have T being int and independently U also being int.

Therefore, we can use the above template method for all the 5 use cases:
```
(int, int)
(float, float)
(long, long)
(double, double)
(long, int)
```
As you can notice, the template is not just restricted to these 5 pairs of arguments. (int, long), (float, int) and any other pair of data 
types will be valid here.

Let's rewrite the previous code to use the template with T and U:
```
#include <bits/stdc++.h>
using namespace std;

template <class T, class U>
T max_num (T firstNumber, U secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}

int main() {
	int firstInt = 5, secondInt = 7;
	float firstFloat = 3.14159, secondFloat = 2.71828;
	long firstLong = 54362478632846, secondLong = 7148342783278;
	double firstDouble = 2.71828182845, secondDouble = 3.1415926535;
	
	cout << max_num(firstInt, secondInt) << endl;
	cout << max_num(firstFloat, secondFloat) << endl;
	cout << max_num(firstLong, secondLong) << endl;
	cout << max_num(firstDouble, secondDouble) << endl;

	//Newly added function calls. All of these are valid and will work
	cout << max_num(firstLong, firstInt) << endl;
	cout << max_num(firstInt, firstLong) << endl;
	cout << max_num(firstDouble, firstFloat) << endl;
	cout << max_num(firstDouble, firstInt) << endl;
}
```
Let's modify the sum function from above to work for 2 different types of parameters as well just like we did from max_num.

*Expected Output*
```
12
5.85987
61510821416124
5.85987
54362478632851
1077567379
5.85987
7.71828
```
```
#include <bits/stdc++.h>
using namespace std;

template <class T, class U>
T sum (T firstNumber, U secondNumber) {
	return firstNumber + secondNumber;
}

int main() {
	int firstInt = 5, secondInt = 7;
	float firstFloat = 3.14159, secondFloat = 2.71828;
	long firstLong = 54362478632846, secondLong = 7148342783278;
	double firstDouble = 2.71828182845, secondDouble = 3.1415926535;
  
	cout << sum(firstInt, secondInt) << endl;
	cout << sum(firstFloat, secondFloat) << endl;
	cout << sum(firstLong, secondLong) << endl;
	cout << sum(firstDouble, secondDouble) << endl;

	//Newly added function calls. All of these are valid and will work
	cout << sum(firstLong, firstInt) << endl;
	cout << sum(firstInt, firstLong) << endl;
	cout << sum(firstDouble, firstFloat) << endl;
	cout << sum(firstDouble, firstInt) << endl;
	return 0;
}

```

As we can see that the template can take both class and primitive data types (int, float, long, etc), it must be obvious that we 
can create a template where a class object is passed as a parameter as well.

*Examples*
```
template <class T>
T max_obj (T firstObject, T secondObject) {
	if (firstObject.some_attribute > secondObject.some_attribute) {
		return firstObject;
	}
	return secondObject;
}
template <class T, class U>
int func (T firstObject, U secondObject) {
	int result = 0;
	//Some logic
	return result;
}
```
*FYI*
In case of templates, we've been calling the functions like this:

```
cout << max_num(firstInt, secondInt) << endl;
```
Here, we are relying on the compiler to deduce the type/class of the arguments and use the right function accordingly.

There is a way in which instead of relying on the compiler to deduce the type/class, we can specify it to the compiler directly.

For the below template function with only a single type/class:
```
template <class T>
T max_num (T firstNumber, T secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
```
We can do like this:

```
cout << max_num<int>(firstInt, secondInt) << endl;
```
For the below template function with more than one template type/class:
```
template <class T, class U>
T max_num (T firstNumber, U secondNumber) {
	if (firstNumber > secondNumber) {
		return firstNumber;
	}
	return secondNumber;
}
```
We can do like this for (int, int):

```
cout << max_num<int, int>(firstInt, secondInt) << endl;
```
For (long, int):

```
cout << max_num<long, int>(firstLong, firstInt) << endl;
```
This is just an FYI. In general, we might not have to do this.

