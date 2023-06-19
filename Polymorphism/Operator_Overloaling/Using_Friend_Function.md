
## It turns out that there are three different ways to overload operators: the member function way, the friend function way, and the normal function way. 

##  [Overloading the subtraction operator (+) or Overloading the subtraction operator (-)](https://www.learncpp.com/cpp-tutorial/overloading-the-arithmetic-operators-using-friend-functions/)

## [ I/O operators - Overloading operator<< or Overloading operator>>](https://www.learncpp.com/cpp-tutorial/overloading-the-io-operators/)
```cpp
#include <iostream>

class Cents
{
private:
	int m_cents {};

public:
	Cents(int cents) : m_cents{ cents } { }

	// add Cents + Cents using a friend function
	friend Cents operator+(const Cents& c1, const Cents& c2);

	// subtract Cents - Cents using a friend function
	friend Cents operator-(const Cents& c1, const Cents& c2);

	int getCents() const { return m_cents; }
};

// note: this function is not a member function!
Cents operator+(const Cents& c1, const Cents& c2)
{
	// use the Cents constructor and operator+(int, int)
	// we can access m_cents directly because this is a friend function
	return c1.m_cents + c2.m_cents;
}

// note: this function is not a member function!
Cents operator-(const Cents& c1, const Cents& c2)
{
	// use the Cents constructor and operator-(int, int)
	// we can access m_cents directly because this is a friend function
	return c1.m_cents - c2.m_cents;
}

int main()
{
	Cents cents1{ 6 };
	Cents cents2{ 2 };
  Cents centsSum{ cents1 + cents2 };
	Cents centsMin{ cents1 - cents2 };
	std::cout << "I have " << centsSum.getCents() << " cents.\n";
  std::cout << "I have " << centsMin.getCents() << " cents.\n";

	return 0;
}
```

Overloading the plus operator (+) is as simple as declaring a function named operator+, giving it two parameters of the type of the operands we want to add, picking an appropriate return type,
and then writing the function.

In the case of our Cents object, implementing our operator+() function is very simple. First, the parameter types: in this version of operator+, we are going to add two Cents objects together, 
so our function will take two objects of type Cents. Second, the return type: our operator+ is going to return a result of type Cents, so that’s our return type.

Finally, implementation: to add two Cents objects together, we really need to add the m_cents member from each Cents object. Because our overloaded operator+() function is a friend of the class,
we can access the m_cents member of our parameters directly. Also, because m_cents is an integer, and C++ knows how to add integers together using the built-in version of the plus operator that 
works with integer operands, we can simply use the + operator to do the adding.


-------------------------------------------------------------------------------------------

## Friend functions can be defined inside the class

**Note:**  Even though friend functions are not members of the class, they can still be defined inside the class if desired:

```cpp
#include <iostream>

class Cents
{
private:
	int m_cents {};

public:
	Cents(int cents) : m_cents{ cents } { }

	// add Cents + Cents using a friend function
        // This function is not considered a member of the class, even though the definition is inside the class
	friend Cents operator+(const Cents& c1, const Cents& c2)
	{
		// use the Cents constructor and operator+(int, int)
		// we can access m_cents directly because this is a friend function
		return c1.m_cents + c2.m_cents;
	}

	int getCents() const { return m_cents; }
};

int main()
{
	Cents cents1{ 6 };
	Cents cents2{ 8 };
	Cents centsSum{ cents1 + cents2 };
	std::cout << "I have " << centsSum.getCents() << " cents.\n";

	return 0;
}
```
---------------------------------------------------------------------------------------------

## Overloading operators for operands of different types

Often it is the case that you want your overloaded operators to work with operands that are different types. For example, if we have Cents(4), we may want to add the integer 6 to this to
produce the result Cents(10).

When C++ evaluates the expression `x + y, x` becomes the first parameter, and `y` becomes the second parameter. When `x` and `y` have the same type, it does not matter if you add `x + y or y + x` 
-- either way, the same version of operator+ gets called. However, when the operands have different types, `x + y` does not call the same function as `y + x.`

For example, `Cents(4) + 6` would call o`perator+(Cents, int)`, and `6 + Cents(4)` would call `operator+(int, Cents)`. Consequently, whenever we overload binary operators for operands of 
different types, we actually need to write two functions -- one for each case. Here is an example of that:

```cpp
#include <iostream>

class Cents
{
private:
	int m_cents {};

public:
	Cents(int cents) : m_cents{ cents } { }

	// add Cents + int using a friend function
	friend Cents operator+(const Cents& c1, int value);

	// add int + Cents using a friend function
	friend Cents operator+(int value, const Cents& c1);


	int getCents() const { return m_cents; }
};

// note: this function is not a member function!
Cents operator+(const Cents& c1, int value)
{
	// use the Cents constructor and operator+(int, int)
	// we can access m_cents directly because this is a friend function
	return c1.m_cents + value;
}

// note: this function is not a member function!
Cents operator+(int value, const Cents& c1)
{
	// use the Cents constructor and operator+(int, int)
	// we can access m_cents directly because this is a friend function
	return c1.m_cents + value;
}

int main()
{
	Cents c1{ Cents{ 4 } + 6 };
	Cents c2{ 6 + Cents{ 4 } };

	std::cout << "I have " << c1.getCents() << " cents.\n";
	std::cout << "I have " << c2.getCents() << " cents.\n";

	return 0;
}
```

**Note** that both overloaded functions have the same implementation -- that’s because they do the same thing, they just take their parameters in a different order.

-------------------------------------------------------------------------------------------

## Another example

```cpp
#include <iostream>

class MinMax
{
private:
	int m_min {}; // The min value seen so far
	int m_max {}; // The max value seen so far

public:
	MinMax(int min, int max)
		: m_min { min }, m_max { max }
	{ }

	int getMin() const { return m_min; }
	int getMax() const { return m_max; }

	friend MinMax operator+(const MinMax& m1, const MinMax& m2);
	friend MinMax operator+(const MinMax& m, int value);
	friend MinMax operator+(int value, const MinMax& m);
};

MinMax operator+(const MinMax& m1, const MinMax& m2)
{
	// Get the minimum value seen in m1 and m2
	int min{ m1.m_min < m2.m_min ? m1.m_min : m2.m_min };

	// Get the maximum value seen in m1 and m2
	int max{ m1.m_max > m2.m_max ? m1.m_max : m2.m_max };

	return { min, max };
}

MinMax operator+(const MinMax& m, int value)
{
	// Get the minimum value seen in m and value
	int min{ m.m_min < value ? m.m_min : value };

	// Get the maximum value seen in m and value
	int max{ m.m_max > value ? m.m_max : value };

	return { min, max };
}

MinMax operator+(int value, const MinMax& m)
{
	// call operator+(MinMax, int)
	return m + value;
}

int main()
{
	MinMax m1{ 10, 15 };
	MinMax m2{ 8, 11 };
	MinMax m3{ 3, 12 };

	MinMax mFinal{ m1 + m2 + 5 + 8 + m3 + 16 };

	std::cout << "Result: (" << mFinal.getMin() << ", " <<
		mFinal.getMax() << ")\n";

	return 0;
}
```

The MinMax class keeps track of the minimum and maximum values that it has seen so far. We have overloaded the `+` operator `3` times, so that we can add two MinMax objects together, or 
add integers to MinMax objects.

**This example produces the result:**

```
Result: (3, 16)
which you will note is the minimum and maximum values that we added to mFinal.
```

Let’s talk a little bit more about how `“MinMax mFinal { m1 + m2 + 5 + 8 + m3 + 16 }”` evaluates. Remember that operator+ evaluates from left to right, so m1 + m2 evaluates first. 
This becomes a call to `operator+(m1, m2)`, which produces the return value `MinMax(8, 15)`. Then `MinMax(8, 15) + 5` evaluates next. This becomes a call to `operator+(MinMax(8, 15), 5),`
which produces return value `MinMax(5, 15)`. Then `MinMax(5, 15) + 8` evaluates in the same way to produce `MinMax(5, 15)`. Then `MinMax(5, 15) + m3` evaluates to produce `MinMax(3, 15)`. And 
finally, `MinMax(3, 15) + 16` evaluates to `MinMax(3, 16)`. This final result is then used to initialize mFinal.

In other words, this expression evaluates as `“MinMax mFinal = (((((m1 + m2) + 5) + 8) + m3) + 16)”,` with each successive operation returning a MinMax object that becomes the left-hand operand 
for the following operator.

-------------------------------------------------------------------------------------

## Overloading operator<<

Overloading operator<< is similar to overloading operator+ (they are both binary operators), except that the parameter types are different.

Consider the expression std::cout << point. If the operator is `<<`, what are the operands? The `left operand` is the `std::cout` object, and the `right` operand is your `Point` class object. 
`std::cout` is actually an object of type `std::ostream`. Therefore, our overloaded function will look like this:

```cpp
// std::ostream is the type for object std::cout
friend std::ostream& operator<< (std::ostream& out, const Point& point);
```

Implementation of operator<< for our Point class is fairly straightforward -- because C++ already knows how to output doubles using `operator<<`, and our members are all doubles, we can simply use `operator<<` to output the member variables of our Point. Here is the above Point class with the overloaded `operator<<`.

```cpp
#include <iostream>

class Point
{
private:
    double m_x{};
    double m_y{};
    double m_z{};

public:
    Point(double x=0.0, double y=0.0, double z=0.0)
      : m_x{x}, m_y{y}, m_z{z}
    {
    }

    friend std::ostream& operator<< (std::ostream& out, const Point& point);
};

std::ostream& operator<< (std::ostream& out, const Point& point)
{
    // Since operator<< is a friend of the Point class, we can access Point's members directly.
    out << "Point(" << point.m_x << ", " << point.m_y << ", " << point.m_z << ')'; // actual output done here

    return out; // return std::ostream so we can chain calls to operator<<
}

int main()
{
  //  const Point point1{2.0, 3.0, 4.0};
    
    Point point1{2.0, 3.5, 4.0};
    Point point2{6.0, 7.5, 8.0};

    std::cout << point1 << ' ' << point2 << '\n';

    return 0;
}
```

This is pretty straightforward -- note how similar our output line is to the line in the print() function we wrote previously. The most notable difference is that `std::cout` has become 
parameter out (which will be a reference to std::cout when the function is called).

The trickiest part here is the return type. With the arithmetic operators, we calculated and returned a single answer by value (because we were creating and returning a new result).
However, if you try to return std::ostream by value, you’ll get a compiler error. This happens because std::ostream specifically disallows being copied.

In this case, we return the left hand parameter as a reference. This not only prevents a copy of std::ostream from being made, it also allows us to `“chain”` output commands together, such as `std::cout << point << std::endl;`

Consider what would happen if our `operator<<` returned void instead. When the compiler evaluates `std::cout << point << '\n',` due to the precedence/associativity rules, it evaluates this expression as `(std::cout << point) << '\n';`. std::cout << point would call our void-returning overloaded `operator<<` function, which returns void. Then the partially evaluated expression becomes: `void << '\n';`, which makes no sense!

By returning the out parameter as the return type instead, `(std::cout<< point) returns std::cout.` Then our partially evaluated expression becomes: `std::cout << '\n';`, which then gets evaluated itself!

Any time we want our overloaded binary operators to be chainable in such a manner, the `left` operand should be returned `(by reference)`. Returning the `left-hand parameter` by reference is okay in this case -- since the` left-hand` parameter was passed in by the calling function, it must still exist when the called function returns. Therefore, we don’t have to worry about referencing something that will go out of scope and get destroyed when the operator returns.


--------------------------------------------------------------------------------------------------

## Overloading operator>>

It is also possible to overload the input operator. This is done in a manner analogous to overloading the output operator. The key thing you need to know is that std::cin is an object of type std::istream. Here’s our Point class with an overloaded operator>>:

```cpp
#include <iostream>

class Point
{
private:
    double m_x{};
    double m_y{};
    double m_z{};

public:
    Point(double x=0.0, double y=0.0, double z=0.0)
      : m_x{x}, m_y{y}, m_z{z}
    {
    }

    friend std::ostream& operator<< (std::ostream& out, const Point& point);
    friend std::istream& operator>> (std::istream& in, Point& point);
};

std::ostream& operator<< (std::ostream& out, const Point& point)
{
    // Since operator<< is a friend of the Point class, we can access Point's members directly.
    out << "Point(" << point.m_x << ", " << point.m_y << ", " << point.m_z << ')';

    return out;
}

std::istream& operator>> (std::istream& in, Point& point)
{
    // Since operator>> is a friend of the Point class, we can access Point's members directly.
    // note that parameter point must be non-const so we can modify the class members with the input values
    in >> point.m_x;
    in >> point.m_y;
    in >> point.m_z;

    return in;
}
```

Here’s a sample program using both the overloaded operator<< and operator>>:

```cpp
int main()
{
    std::cout << "Enter a point: ";

    Point point;
    std::cin >> point;

    std::cout << "You entered: " << point << '\n';

    return 0;
}
```

Assuming the user enters 3.0 4.5 7.26 as input, the program produces the following result:

`You entered: Point(3, 4.5, 7.26)`

****Conclusion****

Overloading `operator<< and operator>>` make it extremely easy to output your class to screen and accept user input from the console.

-------------------------------------------------------------------------------------------------

## Question Take the Fraction class we wrote in the previous quiz (listed below) and add an overloaded operator<< and operator>> to it.

```cpp
#include <iostream>
#include <limits>
#include <numeric> // for std::gcd

class Fraction
{
private:
	int m_numerator{ 0 };
	int m_denominator{ 1 };

public:
	Fraction(int numerator=0, int denominator = 1) :
		m_numerator{ numerator }, m_denominator{ denominator }
	{
		// We put reduce() in the constructor to ensure any new fractions we make get reduced!
		// Any fractions that are overwritten will need to be re-reduced
		reduce();
	}

	void reduce()
	{
		int gcd{ std::gcd(m_numerator, m_denominator) };
		if (gcd)
		{
			m_numerator /= gcd;
			m_denominator /= gcd;
		}
	}

	friend Fraction operator*(const Fraction& f1, const Fraction& f2);
	friend Fraction operator*(const Fraction& f1, int value);
	friend Fraction operator*(int value, const Fraction& f1);

	friend std::ostream& operator<<(std::ostream& out, const Fraction& f1);
	friend std::istream& operator>>(std::istream& in, Fraction& f1);

	void print()
	{
		std::cout << m_numerator << '/' << m_denominator << '\n';
	}
};

Fraction operator*(const Fraction& f1, const Fraction& f2)
{
	return { f1.m_numerator * f2.m_numerator, f1.m_denominator * f2.m_denominator };
}

Fraction operator*(const Fraction& f1, int value)
{
	return { f1.m_numerator * value, f1.m_denominator };
}

Fraction operator*(int value, const Fraction& f1)
{
	return { f1.m_numerator * value, f1.m_denominator };
}

std::ostream& operator<<(std::ostream& out, const Fraction& f1)
{
	out << f1.m_numerator << '/' << f1.m_denominator;
	return out;
}

std::istream& operator>>(std::istream& in, Fraction& f1)
{
	// Overwrite the values of f1
	in >> f1.m_numerator;

	// Ignore the '/' separator
	in.ignore(std::numeric_limits<std::streamsize>::max(), '/');

	in >> f1.m_denominator;

	// Since we overwrite the existing f1, we need to reduce again
	f1.reduce();

	return in;
}

int main()
{
	Fraction f1;
	std::cout << "Enter fraction 1: ";
	std::cin >> f1;

	Fraction f2;
	std::cout << "Enter fraction 2: ";
	std::cin >> f2;

	std::cout << f1 << " * " << f2 << " is " << f1 * f2 << '\n'; // note: The result of f1 * f2 is an r-value

	return 0;
}
```


