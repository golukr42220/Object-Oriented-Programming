****Functors****

[Functors](https://www.learncpp.com/cpp-tutorial/overloading-the-parenthesis-operator/)


Basically, functors are not functions, they are `function objects`. Objects of a class in C++ that defines the `operator()` method (also referred to as call operator or the application operator)
are called functors. Objects of this class look like a function as they end with `()` parenthesis. A functor is advantageous over normal functions as it can support multiple independent states,
one for each functor instance, while functions only support a single state.

 We will take a functor to encode a given integer value and we will create a lambda function to decode the encoded value.

```cpp
#include<bits/stdc++.h>
using namespace std;

// functor defination
class MyFunctor
{
   public:
     int operator()(int x) { return (x * 2 + 42)/2 - 7;}

};
int main()
{
    MyFunctor encoder;
    int x = encoder(20);
    cout << "Encoded message: " << x << "\n";
    // lambda expression
    auto decoder = [](int x){
        return  ((x+7) * 2 - 42)/2;
    };
    cout << "Decoded message: " << decoder(x);
    return 0;
}
```

**Output:**
```
Encoded message: 34
Decoded message: 20
```

- In the example above MyFunctor is a functor. It is a class that extends operator(), and takes x as a parameter. It performs some operations on it to encode the value and returns it.

- To decode the encoded value in x, we use the variable y to store the result. We use a lambda expression for the calculation of the original value.

- Both the functor and lambda expressions are very much similar to each other. The only difference is lambda doesn't have a name and its code is in simplified form. We can use functors at multiple
places as well as they store state whereas lambdas cannot be used more than once.

- Functors are objects of classes so they are a little more complex than lambdas. Also, their implementation lies with the class and it is used in the main() method whereas lambda expression is 
implemented in the main() method itself.

- `Operator()` is also commonly overloaded to implement functors (or function object), which are classes that operate like functions. The advantage of a functor over a normal function is that functors can store data in member variables (since they are classes).

```cpp
#include <iostream>

class Accumulator
{
private:
    int m_counter{ 0 };

public:
    int operator() (int i) { return (m_counter += i); }
};

int main()
{
    Accumulator acc{};
    std::cout << acc(10) << '\n'; // prints 10
    std::cout << acc(20) << '\n'; // prints 30

    return 0;
}
```

**Note** that using our Accumulator looks just like making a normal function call, but our Accumulator object is storing an accumulated value.

You may wonder why we couldn’t do the same thing with a normal function and a static local variable to preserve data between function calls. We could, but because functions only have one 
global instance, we’d be limited to using it for one thing at a time. With functors, we can instantiate as many separate functor objects as we need and use them all simultaneously.

****Conclusion****

- **Operator()** is sometimes overloaded with two parameters to index multidimensional arrays, or to retrieve a subset of a one dimensional array (with the two parameters defining the subset to return). Anything else is probably better written as a member function with a more descriptive name.

- **Operator()** is also often overloaded to create functors. Although simple functors (such as the example above) are fairly easily understood, functors are typically used in more advanced programming topics, and deserve their own lesson.

**Question 1**

Write a class named `MyString` that holds a `std::string`. Overload `operator<<` to output the string. Overload `operator()` to return the substring that starts at the index of the first parameter (as a `MyString`). The length of the substring should be defined by the second parameter.

```cpp
#include <cassert>
#include <iostream>
#include <string>

class MyString
{
private:
	std::string m_string{};

public:
	MyString(const std::string& string = {})
		:m_string{ string }
	{
	}

	MyString operator()(int start, int length)
	{
		assert(start >= 0);
		assert(start + length <= static_cast<int>(m_string.length()) && "MyString::operator(int, int): Substring is out of range");

		return m_string.substr(
			static_cast<std::string::size_type>(start),
			static_cast<std::string::size_type>(length)
		);
	}

	friend std::ostream& operator<<(std::ostream& out, const MyString& s)
	{
		out << s.m_string;

		return out;
	}
};

int main()
{
	MyString s{ "Hello, world!" };
	std::cout << s(7, 5) << '\n'; // start at index 7 and return 5 characters

	return 0;
}
```

**Question 2**

Extra credit: Why is the above inefficient if we need the substring only temporarily (assume you used `std::string::substr` to implement the substring)?

**Solution**

`std::string::substr` returns a `std::string`, which means when we call it, we’re making a copy of part of the source string. Our overloaded operator() uses this to construct a new `MyString`, which contains a `std::string` member, which makes another copy. We then return this `MyString` to the caller, which makes a third copy.

Note: The compiler will probably optimize some of these copies out of existence, but at least one `std::string` (containing the resulting substring) must be kept.

**Question 3**

Extra credit: Implement a member function named substr that returns the same substring as a `std::string_view`.

The following code should run and produce the same result as above:

**Solution**

Hint: std::string::substr returns a std::string. std::string_view::substr returns a std::string_view. Be very careful not to return a dangling std::string_view!.

```cpp
#include <cassert>
#include <iostream>
#include <string>
#include <string_view>

class MyString
{
private:
	std::string m_string{};

public:
	MyString(const std::string& string = {})
		:m_string{ string }
	{
	}

	MyString operator()(int start, int length)
	{
		assert(start >= 0);
		assert(start + length <= static_cast<int>(m_string.length()) && "MyString::operator(int, int): Substring is out of range");

		return m_string.substr(
			static_cast<std::string::size_type>(start),
			static_cast<std::string::size_type>(length)
		);
	}

	std::string_view substr(int start, int length)
	{
		assert(start >= 0);
		assert(start + length <= static_cast<int>(m_string.length()) && "MyString::substr(int, int): Substring is out of range");

		return std::string_view{ m_string }.substr(
			static_cast<std::string_view::size_type>(start),
			static_cast<std::string_view::size_type>(length)
		);
	}

	friend std::ostream& operator<<(std::ostream& out, const MyString& s)
	{
		out << s.m_string;

		return out;
	}
};

int main()
{
	MyString s{ "Hello, world!" };
	std::cout << s.substr(7, 5) << '\n'; // start at index 7 and return 5 characters

	return 0;
}
```

Let’s explore return std::string_view{ m_string }.substr(start, length); further. First, we’re creating a temporary std::string_view of m_string, which is inexpensive and lets us access std::string_view member functions. Next, we call std::string_view::substr on this temporary to get our substring (as a non-null-terminated view of m_string). We then return this view to the caller. Since the std::string_view we return to the caller is still a view of m_string, it is not dangling.

The end result is we create 3 std::string_view instead of 3 std::string, which is more efficient.
