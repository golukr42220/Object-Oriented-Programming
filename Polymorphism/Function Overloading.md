Let's say that we want to create a program where:

- We can find the maximum of two integers
- We can find the maximum of two decimals (double)
- We can find the maximum of three integers
- We can find the maximum of three decimals (double)
Since it is not possible to create functions with the same variable names in C, what we can do here is:

```
int max_two_integers (int firstNumber, int secondNumber) {
	//Logic for finding max of 2 numbers
}
double max_two_decimals (double firstNumber, double secondNumber) {
	//Logic for finding max of 2 numbers
}
int max_three_integers (int firstNumber, int secondNumber, int thirdNumber) {
	//Logic for finding max of 3 numbers
}
double max_three_decimals (double firstNumber, double secondNumber, double thirdNumber) {
	//Logic for finding max of 3 numbers
}
```

Does not look good, right?

C++ was created to allow writing clean, structured code and so it allows us to create functions with the same name (with different parameters).

In C++, we can do something like this:

```
int max_num (int firstNumber, int secondNumber) {
	//Logic for finding max of 2 numbers
}
double max_num (double firstNumber, double secondNumber) {
	//Logic for finding max of 2 numbers
}
int max_num (int firstNumber, int secondNumber, int thirdNumber) {
	//Logic for finding max of 3 numbers
}
double max_num (double firstNumber, double secondNumber, double thirdNumber) {
	//Logic for finding max of 3 numbers
}
```

Let's write a code with 4 functions named `max_num` each with the following logic:

- Find the maximum of two integers
- Find the maximum of two decimals (double)
- Find the maximum of three integers
- Find the maximum of three decimals (double)
- In the main method call these with the following values and print the return values.

```
4 12
1.618 2.71828
4 54 12
3.14159 1.618 2.71828
```
**Expected Output**
```
12
2.71828
54
3.14159
```

```
#include <bits/stdc++.h>
using namespace std;

class Max_number{
	
	public:
	int max_num (int firstNumber, int secondNumber) {
	//Logic for finding max of 2 numbers
		return firstNumber > secondNumber ? firstNumber:secondNumber;
	}
	double max_num (double firstNumber, double secondNumber) {
		//Logic for finding max of 2 numbers
		return firstNumber > secondNumber ? firstNumber:secondNumber;
	}
	int max_num (int firstNumber, int secondNumber, int thirdNumber) {
		//Logic for finding max of 3 numbers
		if(firstNumber > secondNumber and firstNumber>secondNumber)
			  return firstNumber;
		else if(secondNumber > firstNumber and secondNumber >thirdNumber)
			return secondNumber;
		else 
			return thirdNumber;
	}
	double max_num (double firstNumber, double secondNumber, double thirdNumber)     {
		if(firstNumber > secondNumber and firstNumber>secondNumber)
			  return firstNumber;
		else if(secondNumber > firstNumber and secondNumber >thirdNumber)
			return secondNumber;
		else 
			return thirdNumber;
		//Logic for finding max of 3 numbers
	}
};
int main() {
	// your code goes here
	
	Max_number mx;
	cout<<mx.max_num(4, 12)<<endl;
	cout<<mx.max_num(1.618, 2.71828)<<endl;
	cout<<mx.max_num(4, 54, 12)<<endl;
	cout<<mx.max_num(3.14159, 1.618 , 2.71828)<<endl;
	
	return 0;
}
```

This is known as `function overloading`.

Function overloading is the ability to create multiple functions with the same name but different implementations.

What basically happens here is that when the code is getting compiled, the compiler tries to select the correct method for each of function calls based on the arguments being passed (number of arguments and data types of those arguments).

`Note that this is not valid`:

```
int divide (int numerator, int denominator) {
	return numerator/denominator;
}
float divide (int numerator, int denominator) {
	return numerator/((float) denominator);
}
```

Here, even though the implementation and return type is different, the number of parameters and the data types are still the same. That's why it is not valid. Function Overloading allows us to create functions with the same name with different parameters only.

Let's write another code. This time with 3 functions named 'sum' each with the following logic:

- Find the sum of two integers
- Find the sum of two decimals
- Find the sum of two complex numbers.
Since there is no data type for complex numbers, I have already created a class for complex number so that you don't have to.

A complex number is a number which has a real part and an imaginary part represented as:
```
a + bi
```
Here, a and b are numbers. a represents the real part and bi represents the imaginary part.
`i` is the imaginary unit (a suffix used in the imaginary part).

**Example**
```
3 + 5i
```

Here, 3 is the real part and 5i is the imaginary part.

You can read more about complex numbers here if you want to. Though the information mentioned in this section is sufficient for this course.

Sum of two complex numbers is another complex number with the real part as the sum of the real part of the two objects and the imaginary part should be the sum of the imaginary part of the two objects.
```
(a + bi) + (c + di) => (a + c) + (b + d)i
```
**Example**
```
(1 + 3i) + (4 + 8i) => 5 + 11i
```
In the main method, the variables has been created. Call the methods on them and print the return values.

- 7 11
- 3.14 2.718
- Complex Number objects (1 + 3i) and (4 + 8i)
**Expected Output**
```
18
5.858
5 + 11i
```
Use the `getFormatted` method to get the complex number in this format.

```
#include <bits/stdc++.h>
using namespace std;

class ComplexNumber {
private:
	int real;
	int imaginary;
public:
	ComplexNumber (int real, int imaginary) {
		this->real = real;
		this->imaginary = imaginary;
	}
	
	
	int getRealPart () {
		return real;
	}
	
	int getImaginaryPart () {
		return imaginary;
	}
	
	//You can use this method on a ComplexNumber object to get the string value
	//Use case: cout << complexNumber.getFormatted();
	string getFormatted() {
		//to_string is an in-built method to convert a number to string
		//This will return 3 + 7i for real:3 and imaginary:7
		return to_string(real) + " + " + to_string(imaginary) + "i";
	}
};


int main() {
	// your code goes here
	int firstInteger = 7, secondInteger = 11;
	double firstDouble = 3.14, secondDouble = 2.718;
	int firstReal = 1, firstImaginary = 3;
	int secondReal = 4, secondImaginary = 8;
	//Create two ComplexNumber objects using (firstReal and firstImaginary) and (secondReal and secondImaginary)
	
	ComplexNumber complexNumber(firstReal, firstImaginary); 
	
	ComplexNumber complexNumber1(secondReal, secondImaginary); 
	
	//Call and Print the sum methods for firstInteger and secondInteger
	cout << firstInteger + secondInteger<<endl;
	//Call and Print the sum methods for firstDouble and secondDouble
	cout << firstDouble + secondDouble<<endl;
	//Call and Print the sum methods for firstComplex and secondComplex
	 
	  ComplexNumber complexNumber2(complexNumber.getRealPart() +       complexNumber1.getRealPart(), complexNumber.getImaginaryPart() + complexNumber1.getImaginaryPart() );
	 cout<< complexNumber2.getFormatted();
	return 0;
}
```
