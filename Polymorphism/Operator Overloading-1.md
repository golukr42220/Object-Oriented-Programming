We've learned about complex numbers and how to find its sum in the previous section. When we can use + to get the sum of two integers, decimals, etc, why can't we do the same with complex numbers?

Because it is not an in-built data type.

If we do `firstComplexNumber + secondComplexNumber`, the compiler won't understand how to do it because it just knows that these are objects of class ComplexNumber and don't know anything about how to do any operation on them.

So what if we can tell the compiler how to do an operation on it?

C++ allows us to do that through something known as "operator overloading".

Operator overloading is similar to function overloading in a way that instead of the same function name, we have the same operator behaving differently depending on the parameters.

We can assume that the internal implementation of + operator is somethign like this:

```
//Here "operator +" denotes a function which does + operation on the parameters
int operator + (int firstNumber, int secondNumber) {
	//Some internal implementation
}

float operator + (float firstNumber, float secondNumber) {
	//Some internal implementation
}

long operator + (long firstNumber, long secondNumber) {
	//Some internal implementation
}

double operator + (double firstNumber, double secondNumber) {
	//Some internal implementation
}
```

Now let's create an overloaded + operator for class ComplexNumber as well.

This is the sum method that we created in the previous section:

```
ComplexNumber sum (ComplexNumber firstNumber, ComplexNumber secondNumber) {
	int real_part = firstNumber.getRealPart() + secondNumber.getRealPart();
	int imaginary_part = firstNumber.getImaginaryPart() + secondNumber.getImaginaryPart();
	return ComplexNumber(real_part, imaginary_part);
}
```

We can modify the same to do an operator overloading.

Replace the function name from `"sum"` with `"operator +" ` in the below code and hit run.

Note that you need to mention both operator and +.
```
operator +
```
**Expected Output**
```
3+9i
```

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
	
	string getFormatted() {
		//to_string is an in-built method to convert a number to string
		//This will return 3+7i for real:3 and imaginary:7
		return to_string(real) + "+" + to_string(imaginary) + "i";
	}
};

ComplexNumber operator + (ComplexNumber firstNumber, ComplexNumber secondNumber) {
	int real_part = firstNumber.getRealPart() + secondNumber.getRealPart();
	int imaginary_part = firstNumber.getImaginaryPart() + secondNumber.getImaginaryPart();
	return ComplexNumber(real_part, imaginary_part);
}

int main() {
	// your code goes here
	ComplexNumber complexNumber(1, 3), complexNumber2(2, 6);
	cout << (complexNumber + complexNumber2).getFormatted();
	return 0;
}

```

What basically happens here is that when the code is getting compiled, the compiler tries to select the correct implementation for each of the operations based on the arguments (operands) being passed (operated on). There is an implementation present internally for each of the operations that we've been doing until now on in-built data types. So during compilation, the compiler tries to find the correct implementation. If it is unable to find for the set of operands then it gives an error.

Note that operator overloading is not limited to arithmetic operators. Almost all operators including arithmetic, relational, logical, assignment, etc can be overloaded.

Syntax of operator overloading is:

```
return_type operator op (data_type param1, ...) {
}
```

In the above example:

```
ComplexNumber operator + (ComplexNumber firstNumber, ComplexNumber secondNumber) {
	//Addition Logic
}
```
This type of function is known as operator function. The function name is "operator" followed by the operator symbol.

- Let's write a code.
- Given UML diagram for Time class:

![image](https://user-images.githubusercontent.com/45598340/232583944-0c005275-415e-4f3b-9f9f-7152b60315d4.png)

- Overload the greater than operator (>) for two Time Objects.
- Given two time objects timeFirst and timeSecond, timeFirst > timeSecond should return 1 if:
   - timeFirst's hours is greater than timeSecond's hours
   - timeFirst's hours is equal to timeSecond's but timeFirst's minutes is greater
   - timeFirst's hours and minutes are equal to timeSecond's but timeFirst's seconds is greater
- Note that you can use int as the return type for > operator and return 1 if first parameter is greater than second parameter otherwise return 0.
- Do not modify the main method.

**Expected Output**
```
1 0
```

```
#include <bits/stdc++.h>
using namespace std;

class Time{
	int hr;
	int min;
	int sec;
	public:
	Time(int hr,int min, int sec){
		this->hr=hr;
		this->min=min;
		this->sec=sec;
	}
	
	int operator >( Time &t2){
		
		if(this->hr > t2.hr)
			return 1;
		else if(this->hr == t2.hr && this->min > t2.min)
			return 1;
		else if(this->hr == t2.hr && this->min == t2.min && this->sec > t2.sec)
			return 1;
		else 
			return 0;
		
	}
	
};
int main() {
	// your code goes here
	Time t1(15, 10, 35);
	Time t2(12, 45, 55);
	cout << (t1 > t2) << " " << (t2 > t1);
	return 0;
}
```
