****Smart Pointers****

[Read here](https://www.scaler.com/topics/cpp/smart-pointers-in-cpp/)

A Pointer is a variable that holds the address of another variable. To use a pointer you must allocate the memory for that pointer variable. The problem with them is that if you forget to
free the memory held by the pointer will be occupied, for the entire duration the program runs. This is called a `memory leak`.

So the `smart pointer` in C++ gives you a way out of this. When using a smart pointer the smart pointer will free the memory when the variable goes out of scope.

- ****The Problems with Normal Pointers****
`Pointers` are an essential part of programming. It helps to work with a single variable across multiple functions. But all the features come along with a price tag, one you must pay. 
You need to pay extreme attention when working with pointers in an application. You have to track the pointer variable and free the memory carefully it holds after performing all your operations. 
If you fail to do so, the program will steal a bit of memory every time and hold it for the entire duration the program is in execution, resulting in a `memory leak` cause you have lost the 
pointer, and now you don't know where the `block of memory` is.

And if this happens enough times, you will run out of space so quickly and will get an error of memory exception, and you won't know why that happens.

This is a severe issue. One that needs serious consideration, Right ? Our fellow programmers who wanted to free us all from this trouble of freeing the memory held by a pointer variable
came up with a new idea. And their idea worked.

- ****Introduction to Smart Pointers****
Our fellow programmers inspired by the idea of classes, created a particular type of `user-defined` data type (After all, that's what classes are, Right ? : `user-defined` data types). 
The idea from which they developed the `smart` pointers is that a class object destructs itself when the program's control goes out of the scope in which the object was defined. 
So you can free the memory of the pointer in the destructor. It will automatically free the `memory` held by the pointer. These amazing `smart` pointers are defined inside the header file
`<memory>`.

So you have to first import that header into your program to use the smart pointers. And then all you have to do is that the pointer object is defined within the proper scope such that you 
can perform all your desired operations within it. And that is comparatively an easy job.

![image](https://github.com/golukr42220/Object-Oriented-Programming/assets/45598340/ff6c08eb-2e61-4b4c-9f23-c706ecec00aa)

- ****How to Use Smart Pointers****
You might now be asking, "Hey, That sounds cool, how can I make use of it ?", "Will I need to create a separate class for each of the pointers I will use in my program ? That will be a hell 
of a lot of work!"

So here is the answer. In most cases, you will just create a standard raw pointer as you usually do. But then you will pass that standard pointer that you have created to the `smart` pointer 
immediately.

The general syntax for the above-mentioned process might look something like this :

```cpp
smart_pointer_type<data_type>pointer_name(new data_type());
```
Here :

- `new data_type()` is how we will generally create a normal pointer and we immediately pass that to a smart pointer constructor where
- `smart_pointer_type` is the type of `smart` pointer that you wish to use such as `unique_ptr`, `shared_ptr`, or `weak_ptr`.
- and `data_type` is the data type of the pointer variable
- `pointer_name` is the name of the pointer variable.
Now you might be asking but "how do I access the data that the pointer points to ?"

You can just access the data held by the `smart` pointer as you would do with the standard pointers. You can use the `"*" and "->"` operators as you usually do on a standard pointer and 
access the data encapsulated in the `smart` pointer. This is achieved with the help of the operator overloading feature of the C++ programming language.

****Types of Smart Pointer****

**1. Unique_ptr**

**2. shared_ptr**

**3. weak-ptr**

**4. auto_ptr**

This is a fantastic idea, but there are simply many use cases for this idea. The C++ programming developers developed so many different Smart pointers, everything slightly different 
from the previous one. Let's talk about the most common ones and see how and where you can use them.

- **1. Unique_ptr**

The `unique_ptr` is one of the most common smart pointers. If you are not sure which smart pointer to use, this is the pointer you should go with.

- The `unique_ptr`, as the name suggests, holds a unique pointer, i.e., the memory held by this pointer cannot be represented by any other pointer. This will be the only pointer that holds
that memory. You will not be allowed to create another copy of the pointer. While the memory held by the pointer cannot be copied or shared by any other pointer, you can always move it to a 
`new` pointer. This can be achieved by the `move()` function of the C++ Standard Template Library.

- **std::unique_ptr** is the C++11 replacement for `std::auto_ptr`. It should be used to manage any dynamically allocated object that is not shared by multiple objects. That is, `std::unique_ptr` should completely own the object it manages, not share that ownership with other classes. `std::unique_ptr` lives in the <memory> header.
    
![image](https://github.com/golukr42220/Object-Oriented-Programming/assets/45598340/1d865de6-0b6e-4a28-a9aa-5424d7c920ae)

Syntax :
```cpp
unique_ptr<Foo> p1(new Foo);
```
The syntax given above explains how to create a unique pointer `p1` for a class called `FOO`. Here replace `FOO` the data type that the memory would hold, and `p1` with the name of the pointer 
you desire.

**Example :**

```cpp
#include <memory>
using namespace std;

void my_func()
{
    unique_ptr<int> ptr(new int(13));
    int x = 45;
    // ...
    // create a new unique_ptr ptr2 and move the ownership of ptr to ptr2
    unique_ptr<int> ptr2 = move(ptr);
    // ptr is now empty
    if (x == 45)
        return; // no memory leak anymore!
    // ...
    // you don't have to delete the pointer variable ptr.
    // the memory will automatically be freed after the return statement.
}

int main()
{
    my_func();
    return 0;
}
```

**Explanation :**

In this example given above, we create a `new` pointer variable ptr for an integer value of `13` within the function called `my_func()`. Then we perform some operations and then `move` 
the `unique` pointer to a new variable called `ptr2`. And then we check a condition called `x=45` and when it evaluates to be true we just return from the function.
We don't bother to `free` the memory we created, because that will be automatically taken care of by the `smart` pointer.

****Another Example****
    
```cpp
#include <iostream>
#include <memory> // for std::unique_ptr

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};

int main()
{
	// allocate a Resource object and have it owned by std::unique_ptr
	std::unique_ptr<Resource> res{ new Resource() };

	return 0;
} // res goes out of scope here, and the allocated Resource is destroyed
```

Because the std::unique_ptr is allocated on the stack here, it’s guaranteed to eventually go out of scope, and when it does, it will delete the Resource it is managing.

Unlike `std::auto_ptr, std::unique_ptr` properly implements move semantics.

```cpp
    #include <iostream>
#include <memory> // for std::unique_ptr
#include <utility> // for std::move

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};

int main()
{
	std::unique_ptr<Resource> res1{ new Resource{} }; // Resource created here
	std::unique_ptr<Resource> res2{}; // Start as nullptr

	std::cout << "res1 is " << (res1 ? "not null\n" : "null\n");
	std::cout << "res2 is " << (res2 ? "not null\n" : "null\n");

	// res2 = res1; // Won't compile: copy assignment is disabled
	res2 = std::move(res1); // res2 assumes ownership, res1 is set to null

	std::cout << "Ownership transferred\n";

	std::cout << "res1 is " << (res1 ? "not null\n" : "null\n");
	std::cout << "res2 is " << (res2 ? "not null\n" : "null\n");

	return 0;
} // Resource destroyed here when res2 goes out of scope
```
  
**This prints:**
```
Resource acquired
res1 is not null
res2 is null
Ownership transferred
res1 is null
res2 is not null
Resource destroyed
```
    
Because `std::unique_ptr` is designed with move semantics in mind, copy initialization and copy assignment are disabled. If you want to transfer the contents managed by `std::unique_ptr`, you must use move semantics. In the program above, we accomplish this via `std::move` (which converts res1 into an r-value, which triggers a move assignment instead of a copy assignment).

****Accessing the managed object****

`std::unique_ptr` has an overloaded `operator*` and `operator->`that can be used to return the resource being managed. `Operator*` returns a `reference` to the managed resource, and `operator->` returns a `pointer`.

Remember that `std::unique_ptr` may not always be managing an object -- either because it was created empty (using the default constructor or passing in a nullptr as the parameter), or because the resource it was managing got moved to another `std::unique_ptr`. So before we use either of these operators, we should check whether the std::unique_ptr actually has a resource. Fortunately, this is easy: `std::unique_ptr` has a cast to bool that returns true if the `std::unique_ptr` is managing a resource.
    
 ```cpp
    #include <iostream>
#include <memory> // for std::unique_ptr

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
	friend std::ostream& operator<<(std::ostream& out, const Resource &res)
	{
		out << "I am a resource";
		return out;
	}
};

int main()
{
	std::unique_ptr<Resource> res{ new Resource{} };

	if (res) // use implicit cast to bool to ensure res contains a Resource
		std::cout << *res << '\n'; // print the Resource that res is owning

	return 0;
}
```
    
**This prints:**
```
Resource acquired
I am a resource
Resource destroyed
```
    
In the above program, we use the overloaded `operator*` to get the Resource object owned by `std::unique_ptr` res, which we then send to `std::cout` for printing.


------------------------------------------------------------------------------------------------------------------------------------
- ****2. Shared_ptr****

The `unique_ptr` will not always be enough for all your use cases. You might have some time required to make a copy of the pointer to pass it to another function. So to help you in those situations,
the `shared_ptr` was developed.

The `shared_ptr` allows you to make a `copy` of the pointer. It will hold the memory until all the pointer holding that memory gets out of scope. This is done by maintaining a `reference counter`.

The `Reference counter` will hold the `count` of pointers pointing to that memory location. The destructor will check the `reference counter` and `free` the memory only if the `reference counter` 
value is `1`, i.e., only the current pointer is pointing to that memory. You can also revoke the ownership you hold over the memory that the pointer holds with the reset method. In that case, 
the `reference count` will be decreased by `1`, representing that there is one less owner for that memory location. At any point in time if you need to know the number of pointers pointing to a
location you can use the `use_count()` function to get that.

![image](https://github.com/golukr42220/Object-Oriented-Programming/assets/45598340/92e7b9f0-8501-42d7-99a1-436bfe85846f)

**Syntax :**

```cpp
shared_ptr<Foo> p1(new Foo);
```

The syntax given above explains how to create a shared pointer `p1` for a class called `FOO`. Here replace `FOO` the data type that the memory would hold, and `p1` with the name of the pointer 
you desire.

**Example :**

```cpp
#include <memory>
#include <iostream>
using namespace std;

class Articles
{
    public:
    char title[30];
    // Implementation of the Articles class.

}

int main()
{

    shared_ptr<Articles> P1(new Articles());
    // Initialise the pointer P1 as you would normally do with a normal pointer
    // This'll print the title of the article
    cout << P1->title << endl;

    shared_ptr<Articles> P2;
    P2 = P1;

    // This'll print also print the same title as printed by P1
    cout << P2->title << endl;
    // You can print the number of pointers pointing to that location with the help of the use_count() method
    cout << P1.use_count() << endl;
    // This'll print 2 as Reference Counter is 2
    // The memory will only be freed when both the pointers P1 and P2 have gone out of scope or
    //released the ownership of the location.

    return 0;
}
```

**Explanation :**

In this example given above, we create `2` shared pointers named `p1` and `p2` for a class called Articles. And we work with them. The memory gets freed only when both the variables `p1` and `p2`
goes out of scope.

- ****Another Example****

 Unlike std::unique_ptr, which is designed to singly own and manage a resource, `std::shared_ptr` is meant to solve the case where you need multiple smart pointers co-owning a resource.
    
 This means that it is fine to have multiple `std::shared_ptr` pointing to the same resource. Internally, `std::shared_ptr` keeps track of how many `std::shared_ptr` are sharing the resource. As long as at least one `std::shared_ptr` is pointing to the resource, the resource will not be deallocated, even if individual `std::shared_ptr` are destroyed. As soon as the last `std::shared_ptr` managing the resource goes out of scope (or is reassigned to point at something else), the resource will be deallocated.

```cpp
#include <iostream>
#include <memory> // for std::shared_ptr

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};

int main()
{
	// allocate a Resource object and have it owned by std::shared_ptr
	Resource* res { new Resource };
	std::shared_ptr<Resource> ptr1{ res };
	{
		std::shared_ptr<Resource> ptr2 { ptr1 }; // make another std::shared_ptr pointing to the same thing

		std::cout << "Killing one shared pointer\n";
	} // ptr2 goes out of scope here, but nothing happens

	std::cout << "Killing another shared pointer\n";

	return 0;
} // ptr1 goes out of scope here, and the allocated Resource is destroyed
```
    
**This prints:**

```
Resource acquired
Killing one shared pointer
Killing another shared pointer
Resource destroyed
```

In the above code, we create a dynamic Resource object, and set a `std::shared_ptr` named `ptr1` to manage it. Inside the nested block, we use the copy constructor to create a second `std::shared_ptr (ptr2)` that points to the same Resource. When `ptr2` goes out of scope, the Resource is not deallocated, because `ptr1` is still pointing at the Resource. When `ptr1` goes
out of scope, `ptr1` notices there are no more `std::shared_ptr` managing the Resource, so it deallocates the Resource.

- **Note** that we created a second shared pointer from the first shared pointer. This is important. Consider the following similar program:
   
```cpp
#include <iostream>
#include <memory> // for std::shared_ptr

class Resource
{
public:
	Resource() { std::cout << "Resource acquired\n"; }
	~Resource() { std::cout << "Resource destroyed\n"; }
};

int main()
{
	Resource* res { new Resource };
	std::shared_ptr<Resource> ptr1 { res };
	{
		std::shared_ptr<Resource> ptr2 { res }; // create ptr2 directly from res (instead of ptr1)

		std::cout << "Killing one shared pointer\n";
	} // ptr2 goes out of scope here, and the allocated Resource is destroyed

	std::cout << "Killing another shared pointer\n";

	return 0;
} // ptr1 goes out of scope here, and the allocated Resource is destroyed again
```
    
**This program prints:**
```
Resource acquired
Killing one shared pointer
Resource destroyed
Killing another shared pointer
Resource destroyed
```
    
and then crashes (at least on the author’s machine).

The difference here is that we created two` std::shared_ptr` independently from each other. As a consequence, even though they’re both pointing to the same Resource, they aren’t aware of each other. When `ptr2` goes out of scope, it thinks it’s the only owner of the Resource, and deallocates it. When `ptr1` later goes out of the scope, it thinks the same thing, and tries to delete the Resource again. Then bad things happen.

Fortunately, this is easily avoided: if you need more than one `std::shared_ptr` to a given resource, copy an existing `std::shared_ptr`.

**Best practice**

Always make a copy of an existing `std::shared_ptr` if you need more than one `std::shared_ptr` pointing to the same resource.
    
--------------------------------------------------------------------------------------------------------------

- ****3. Weak_ptr****

The `weak_ptr` is much like the `shared_ptr`. The only difference is that if you created a `weak_ptr` to a `shared_ptr`, the `reference count` would not increase.

So The Smart Pointer will free the memory irrespective of whether the `weak_ptr` is still in scope. The Programmer can use this if you want to check whether the `shared_ptr` still holds 
the memory or not.

**Syntax :**
```cpp
weak_ptr<Foo> p1(new Foo);
```

The syntax given above explains how to create a weak pointer `p1` for a class called `FOO`. Here replace `FOO` the data type that the memory would hold, and `p1` with the name of the pointer 
you desire.

**Example :**

For this, we can just use the same example that we used for understanding the `Shared_ptr`. Because both of them are very much similar.

```cpp
#include <memory>
#include <iostream>
using namespace std;

class Articles
{
public:
    char title[30];


    // Implementation of the Articles class.

}

int main()
{

    shared_ptr<Articles> P1(new Articles());
    // Initialise the pointer P1 as you would normally do with a normal pointer
    // This'll print the title of the article
    cout << P1->title << endl;

    weak_ptr<Articles> P2;
    P2 = P1;

    // This'll print also print the same title as printed by P1
    cout << P2->title << endl;
    // You can print the number of pointers pointing to that location with the help of the use_count() method
    cout << P1.use_count() << endl;
    // This'll print 1 as the weak pointer is not counted as an owner.
    // The memory gets released once the pointer P1 releases it or goes out of scope.

    return 0;
}
```

**Explanation :**

In this program, we create a `shared` pointer variable named `p1` for the Articles class. Then we perform some operations with the pointer `p1`. Then we create a new `weak pointer` named `p2` 
and assign `p1` to it. We can work with `p2` the same way that we worked with `p1`. But the only difference is that when `p1` goes out of scope the memory gets freed and after that,
we can no longer access the memory with the pointer `p2`.

-------------------------------------------------------------------------------------------

- ****4. Auto_ptr****

The `auto_ptr` is the first implementation of the `smart` pointers. This was similar to that of the `unique_ptr`. However, when the new standard was defined in C++ 11, The `auto_ptr` was 
dropped and replaced with the `three-pointers` mentioned above.

**Node:-** Using the `auto_ptr` in your program is not recommended because it has design flaws. So please use the other pointers mentioned above.

Since this is deprecated and also a bad practice the example for this pointer is omitted in this article.


-------------------------------------------------------------------------------------------

****Guidelines to Follow When Working with Smart Pointers in C++****

- Always try to use smart pointers in C++ because it is better to be sure that you won't be running out of memory due to a `memory leak`.
- Use `Unique_ptr` if you are not sure to use which of the smart pointer to use.
- Use `Shared_ptr` only when dealing with multiple pointers or threads where you will need to share the location.
- If you want just to examine an object and not gonna work with it on any serious level, you can then use the `Weak_ptr`.
- Try to reduce the usage of raw pointers as much as possible, and when you do, make sure that you properly free the memory held by the pointer.

****Conclusion****

- Smart pointers in C++ programming is an idea to compensate for the absence of garbage collectors.
- With Smart Pointers in C++, you can forget about the nightmare of running out of memory because of memory, cause you forgot to free the memory after the execution of a program.
- There are three common types of Smart Pointers in C++ they are Unique_ptr, Shared_ptr, and Weak_ptr.
- Use `Unique_ptr` when not sure which of the smart pointer to use.
- Use `Shared_ptr` only when you need to share the pointer to other functions or threads.
- Use `Weak_ptr` when you don't want the memory to be held by the pointer just for this one pointer.
- Never use `Auto_ptr`. It is always better to use the updated and widely used Smart Pointers.
- Try to reduce the usage of standard raw pointers as much as possible in your programs.


