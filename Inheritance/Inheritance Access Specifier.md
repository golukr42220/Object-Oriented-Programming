While learning inheritance, we have been using public before the name of the base class while extending it like this:

```
class DerivedClass: public BaseClass
```

Here, public is an inheritance access specifier. By using public inheritance access specifier, we are able to maintain the original access specifier of the members of the base class in the derived class.

Let's see what will happen if we use protected inheritance access specifier like this:

```
class ProtectedDerivedClass: protected BaseClass
```
If we do this then all the public and protected class members of the BaseClass will become protected members of the DerivedClass instead of the original access specificity.

Let's see what will happen if we use private inheritance access specifier like this:

```
class PrivateDerivedClass: private BaseClass
```
If we do this then all the public and protected class members of the BaseClass will become private members of the DerivedClass instead of the original access specificity.

- Note that the private members won't be accessible in the derived class irrespective of the inheritance access specificity.
