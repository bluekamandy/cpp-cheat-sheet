# C++ Cheat Sheet

This document is a cheat sheet I created for myself but perhaps it will be useful to others. I will continue to work on it as I learn more about CMake. If you see mistakes or have suggestions, please feel free to contact me at masood@masoodkamandy.com.

## Barebones Main.cpp

No command line arguments:

```c++
int* main()
{
	return 0;
}
```

With command line arguments:

```c++
int main(int argc, char *argv[])
{
	if (argc < 2) // argv[0] is the name of the program.
	{
		LOG("hello world");
  }
	return 1;
}
```

## Common Includes

`#include <iostream>` - to log out to console.

`#include <fstream>` - to read and write to files.

`#include <string>` - the standard library's string header

`#include <cmath>` - for access to math functions (including trig and power)

`#include <cstdlib>` - Many useful things like `abs()` for absolute value and for `atof()` to convert strings to doubles (for dealing with arguments from the command line).

`#include <ctime>` - C++ inherits the date and time library from C. ([Here is some good documentation for this](https://www.tutorialspoint.com/cplusplus/cpp_date_time.htm).)

## Handy Macros

If you come from another language where printing to the console is less verbose, this may be useful:

```c++
#define LOG(msg) \
    std::cout << msg << std::endl
```

The `\` is just for line breaks in macros.

## C++ is Not Type Safe

Coming from Java, C#, or Swift you may be used to what are called type safe languages that offer guards against storing the wrong type of data into a data structure.

Like its predecessor C, C++ does not do this. There are no guard rails. The criticism of non-type-safe languages is that they tend to lead to problems down the line where conversions happen implicitly and with unexpected results. Because of this it's important to know how implicit conversions work.

[Here's a resource](https://en.cppreference.com/w/cpp/language/operator_arithmetic#Conversions) that describes the kinds of implicit conversions that happen in C++.

Here's an examle. I've created a variable and I'd like to store a number in it. Here are 3 implicit conversions that might happen depending on how I write the code:

```c++
float myVariable;

// No conversion. Float is stored as a float.
myVariable = 1.0f;

// Conversion from int to float. If either operand is a float, float always wins.
myVariable = 1;

// Conversion from double to float. Floating point numbers without an f at the end are doubles by default.
myVariable = 1.0;
```

## Header Files

C++ takes all of our code and compiles it as if it were a single document. Because of this, it might be possible for multiple includes to duplicate dependencies. A great way of preventing this is using the following pattern:

```c++
#pragma once

// Includes here

#ifndef _NAMEOFCLASS_H_
#define _NAMEOFCLASS_H_

// Code here

#endif
```

## Difference between struct and class

There is very little difference. By default, **all members of a struct are public**. Classes require you to explicitely declare private vs public members.

## Classes in Header Files

```c++
class MyClass
{
public:
  	// Default constructor
    MyClass();
		// Specialized constructors go here
  	MyClass(int a, int b, int c);
	
		// Any public data/members your class will need should be declared here.
  	int a, b, c;

  	// Any methods your class needs go here.
    void myFunction();

private:
    void privateMethod();
};
```

## Structs

Structs have their members public by default, so they're meant to be slightly more simple than classes.

```c++
struct Color
{
    double r, g, b;

    Color();
    Color(double r, double g, double b);
};
```

## Classes in Source Files

All constructors and methods need to be declared separately in the source (.cpp) file.

```c++
MyClass::MyClass() {}

MyClass::MyClass(int a, int b, int c)
    : a(a), b(b), c(c) // This area assigns the the data coming in to the class's members.
{
    // Anything you need to do in the constructor.
}

```

## Header Only Classes i.e. Template Classes

Here's a good example of one that maps a value from one domain to another.

```c++
template <class T>
inline T map(T value, T low1, T high1, T low2, T high2)
{
		float slope = (value - low1) / (high1 - low1);
		return slope * (high2 - low2) + low2;
}
```

`inline` means that it will compile fully wherever it is used. This can improve performance for some functions.

`T` is the template syntax and will change to whatever type you put in. The limitation of this is that you can only use a single type in functions like this.

## Ternary Operator

It's often useful to use conditional boolean logic when assigning something. The ternary operator is good for this, though it can be difficult to remember and difficult to read. I like to use it, but the compiler would treat a simple if statement in the way, so it offers no performance enhancement.

```
<condition> ? <true-case-code> : <false-case-code>;
```

[Source](https://www.cprogramming.com/reference/operators/ternary-operator.html)