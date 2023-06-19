
It turns out that there are three different ways to overload operators: the member function way, the friend function way, and the normal function way. 

##  [Overloading the subtraction operator (+) or Overloading the subtraction operator (-)](https://www.learncpp.com/cpp-tutorial/overloading-the-arithmetic-operators-using-friend-functions/)

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
