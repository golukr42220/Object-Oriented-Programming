****Functors****

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
