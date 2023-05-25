****Lambda Function****

[Lambda Function](https://www.scaler.com/topics/cpp/lambda-function-cpp/)

**Lambda** function in C++ are in-place functions that can be written for shortcode snippets that are not to be reused so no need to name them as well. It was introduced in Modern C++ beginning 
from C++11. Writing Lambda expression instead of the function wherever necessary makes the code compact, clean, and easy to understand.

We can define lambda expression locally where we want to pass it to a function as an argument or where we are required to call it. They are anonymous function objects which are used widely for
writing in-line functions.

```cpp
#include<bits/stdc++.h>
using namespace std;

int main()
{
    // user input for password
    cout << "Enter Passcode" << endl;
    // actual password
    string password = "me";
    string uspw;
    cin >> userpw;
    
    // lambda function to check if the password entered is correct
    // capturing both passwords in capture claue
    auto checkPasscode = [userpw, &password](){
        if(userpw.compare(password)==0){
            cout << "Login Successful";
        }else{
            cout << "Incorrect password";
        }
    };
    checkPasscode();
}
```
**Output:**
```
Enter Passcode
me
Login Successful
```

In the example above, we are passing userpw by value and password by reference. It returns Login Successful as the password matches user input. checkPasscode is a name given to the lambda 
expression. It stores no value as our lambda expression doesn't return anything.

```cpp
#include <iostream>

using namespace std;

int main()
{
    auto add = [](auto a, auto b){
        return a+b;
    };
    cout << add(56,89) << endl;
    cout << add(90.98, 65.42) << endl;
}
```

**Output:**
```
145
156.4
```

In the above example, add is a lambda expression. auto before add gives a name to the lambda expression. Thus, using this name we can call it multiple times. auto before a and b parameters 
allows us to pass data of different types. The compiler deduces the type of data based on the data provided.

- **Lambda and the STL**

STL algorithms operate on container elements like `vectors`, `arrays`, `stacks`, etc. using function objects, these function objects are passed as an argument to the algorithms.

**For example:**

```cpp
#include <bits/stdc++.h>
#include <string>
using namespace std;

int main()
{
    // string array to store colors
    std::string colors[4] = {"Magenta", "Beige", "Cyan", "Lavender"};  
    
    // for_each loop that checks if the length of the name of the color is greater than or equal to 5. 
    // prints the colors that satifies the condition.
    for_each(colors, colors+4, [](std::string i)
    {
        if(i.length()>=5)
            std::cout << i << " ";
    });
    
    
    cout << endl;
}
```

**Output:**
```
Magenta Beige Lavender 
The above code prints the colors whose length is greater than or equal to 5.
```
In the example above we are passing an anonymous function object with implementation directly to the STL function. Without Lambda expression, we will have to create a function class, 
the object of that function class and then we can pass a single line function to the STL. Rather lambda not only makes the code compact but also reduces the complexity of the program with 
the same functionality.

Applications of Lambdas with Appropriate Example
Applications of lambda expression:

- 1. Lambda expressions are lightweight, readable, and compact.
- 2. They improve the locality of the code, functors are written far away from the place they are called.
- 3. Lambdas allow generic implementation of the function i.e. we can define a general function and use it for multiple data types like int, float, double, etc.
- 4. Using lambda we can overload the same function multiple times in the main method itself.
- 5. Lambdas are good at storing the temporary state of a variable.
- 
Let us take an example of a list of employees, we will check the hours they worked a week. If they worked more than `68` hours, then we will add incentives of `20` percent on their salary.

```cpp
#include <iostream>

using namespace std;

int main()
{
    // [i][0] will store id of employee
    // [i][1] will store working hours a week
    // [i][2] will store the salary per month
    int employee[5][3] = {
    
        {1001, 63, 25000},
        {1002, 69, 30000},
        {1003, 80, 23000},
        {1004, 73, 40000},
        {1005, 50, 20000},
    };
    
    // Immediately invoked a C++ lambda
    [&employee]() mutable {
        for(int i = 0; i < 5 ; i++){
            if(employee[i][1]>68){
                employee[i][2]+=(0.2*employee[i][2]);
            }
        }
    }();
     // prinitng final details
    cout << "Employee id\tHours worked\tAmount+Incentive" << endl;
    for(int i =0;i<5;i++){
        for(int j =0; j<3;j++){
            cout << employee[i][j] << "\t" << "\t";
        }
        cout << endl;
    }
}
```

**Output:**

```
Employee id	Hours worked	Amount+Incentive
1001		63		25000		
1002		69		36000		
1003		80		27600		
1004		73		48000		
1005		50		20000		 
```

- `employee` is an array, the first column stores the employee id, the second column stores the number of hours they worked, and the third-row stores the initial salary.
- 
- Using lambda expression we will check the hours worked by each `employee` and add incentives if the working hours are more than 68 hours a week. We are capturing employee by the reference, 
 the lambda expression doesn't take any parameters and it also doesn't return any value. It makes changes to the employee table thus mutable keyword is used.

- ****Conclusion****

- Lambda expressions are short code snippets that are not to be reused and act as an anonymous function.
- Lambda expression has various components such as capture clause, parameters, mutable and auto keywords, exceptions, return type, etc.
- The auto keyword enables us to assign a name to a lambda expression and also to make generic lambda.
- In the capture clause creation and initialization of variables can be done. To edit these variables we use the mutable keyword.
- Lambda expressions and functors are very much similar to each other. Lambdas are created for single-use without a name and in the main method, unlike functors.
- Lambda's are written while implementing STL algorithms in C++. They make the code readable and compact.
