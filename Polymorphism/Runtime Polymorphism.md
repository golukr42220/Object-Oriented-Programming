In case of function overriding, the actual function that is supposed to be called is not known during compile-time and can only be deduced while the code is being executed (at run-time). Hence, it is used to achieve runtime polymorphism.

This is also known as dynamic dispatch where dispatch means finding the correct function to call. It is done dynamically (at run-time) hence the prefix "dynamic".

The compiler does some work which is then used to select the correct function during run-time. Since the concept has more to do with internal implementation and not application-level implementation, you might not be able to retain it after a while. Focus on understanding the concept and revise it when you need to.

You can read more about the internal workings in this amazing article if you want to.
