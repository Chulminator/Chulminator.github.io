---
title: Lambda expressions
author: chulmin
date: 2024-04-25 00:00:00 -0500
categories: [Programming language, C++]
tags: [lambda expression, c++]
---



## Lambda expressions

Lambda expressions are advantageous for defining concise functions or function objects without the necessity of defining a separate function or function object. They are commonly employed to define functions required only at a specific location in the code, offering a more localized and flexible approach to defining functionality compared to global function definitions.

In this example, the performOperation function takes a function as an input. By using a lambda expression, the input function can be defined locally.
```cpp
#include <iostream>
#include <functional> 

void performOperation(double x, double y, const std::function<double(double, double)>& operation) {
    double result = operation(x, y);
    std::cout << "Operation result: " << result << std::endl;
}

int main() {
    performOperation(5., 3.1, [](double a, double b) -> double {
        return a * b; // Lambda function that multiplies two numbers
    });
    
    performOperation(2.3, 4.7, [](double a, double b) -> double {
        return a + b; // Lambda function that adds two numbers
    });

    return 0;
}

```
{: file='Lambda expression'}

Lambda expressions take the form of [] () -> {}, with each part being called the introducer, parameters, return type, and statements.
- []: Introducer (capture clause) specifies how the lambda expression captures external variables.
- (): Parameter defines the inputs that the lambda function will receive. (can be ommited)
- ->: Return type is indicated using the "->" keyword followed by the return type. (can be ommited)
- {}: Statement consists of the executable code enclosed within the curly braces. 

In this example, input parameters and return values are omitted since they are not present.
```cpp
#include <iostream>

int main() {
    auto printThanks = []{
        std::cout << "Thanks" << std::endl;
    };
    printThanks();
    return 0;
}
```
{: file='Lambda expression1'}

## Introducer

In an introducer, you can specify which variables are included from outside the function. There are two operators: = and &. The = operator copies variables, while the & operator refers to variables. For example,

```cpp
#include <iostream>

int main() {
    int a = 5;
    int b = 10;

    // Capture 'a' by value and 'b' by reference
    auto lambda1 = [a, &b]() {
        // a++; // a is a read-only variable
        b++; // Increment the captured 'b' by reference
        std::cout << "lambda1" << std::endl; 
        std::cout << "a: " << a << std::endl;
        std::cout << "b: " << b << std::endl;
    };
    
    auto lambda2 = [=]() { // Copy all the variables
        // a++; // a is a read-only variable
        // b++; // b is a read-only variable
        std::cout << "lambda2" << std::endl; 
        std::cout << "a: " << a << std::endl;
        std::cout << "b: " << b << std::endl;        
    };
    
    auto lambda3 = [&]() { // Refer all the variables
        a++; // a is a read-only variable
        b++; // Increment the captured 'b' by reference
        std::cout << "lambda3" << std::endl; 
        std::cout << "a: " << a << std::endl;
        std::cout << "b: " << b << std::endl;
    };

    // Call the lambda function
    lambda1();
    lambda2();
    lambda3();

    // Print the updated values of 'a' and 'b'
    
    std::cout << "main" << std::endl; 
    std::cout << "a: " << a << std::endl; // Output: 5 (unchanged)
    std::cout << "b: " << b << std::endl; // Output: 11 (changed)

    return 0;
}
```
{: file='Introducer'}



## Auto

When "auto" is used to declare a variable, the compiler determines the variable's type automatically based on the type of the expression used to initialize it. However, caution should be exercised when using "auto" as its overuse can make it challenging to ascertain the type of variables during debugging and can also compromise readability.

```cpp
auto x = 10; // int
auto pi = 3.14; // double
auto name = "John"; // const char*
```
{: file='auto'}

