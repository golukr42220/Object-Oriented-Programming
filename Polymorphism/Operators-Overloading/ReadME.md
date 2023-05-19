**Operator overloading**

- Three way we can overload Operator

**1-** Overloading the arithmetic operators using friend functions
**2-** Overloading operators using normal functions
**3-** Overloading operators using member functions

[Learn here in details](https://www.learncpp.com/)

The following rules of thumb can help you determine which form is best for a given situation:

- If you’re overloading assignment (=), subscript ([]), function call (()), or member selection (->), do so as a member function.
- If you’re overloading a unary operator, do so as a member function.
- If you’re overloading a binary operator that does not modify its left operand (e.g. operator+), do so as a normal function (preferred) or friend function.
- If you’re overloading a binary operator that modifies its left operand, but you can’t add members to the class definition of the left operand (e.g. operator<<, which has a left operand of type ostream), do so as a normal function (preferred) or friend function.
- If you’re overloading a binary operator that modifies its left operand (e.g. operator+=), and you can modify the definition of the left operand, do so as a member function.
