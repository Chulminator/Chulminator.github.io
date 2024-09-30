---
title: Function template
author: chulmin
date: 2024-04-24 00:00:00 -0500
categories: [Programming language, C++]
tags: [Function template, c++]
---



## Function template

Templates in C++ are a powerful feature used to generalize data types or function behaviors. They allow to create code that can operate on different data types without specifying the exact data type until the code is used. This enhances code reusability and enables the writing of algorithms that can work with various data types.

Taking the example from the [previous post](https://chulminator.github.io/posts/Class-and-Struct/), this illustration stores a 2-dimensional vector and utilizes operators to compare their sizes. Previously, the variable type was fixed to integers, but now it is more flexible and can be reused for different data types.

```cpp
#include <iostream>

template <typename T>
struct Vector2D {
    T x;
    T y;

    // Constructor
    Vector2D(T x_val, T y_val) : x(x_val), y(y_val) {}

    // Overloading the less than operator (<)
    bool operator<(const Vector2D<T>& other) const {
        return (x * x + y * y) < (other.x * other.x + other.y * other.y);
    }

    // Overloading the less than or equal to operator (<=)
    bool operator<=(const Vector2D<T>& other) const {
        return (x * x + y * y) <= (other.x * other.x + other.y * other.y);
    }

    // Overloading the equal to operator (==)
    bool operator==(const Vector2D<T>& other) const {
        return (x == other.x) && (y == other.y);
    }

    // Overloading the not equal to operator (!=)
    bool operator!=(const Vector2D<T>& other) const {
        return !(*this == other);
    }

    // Overloading the greater than or equal to operator (>=)
    bool operator>=(const Vector2D<T>& other) const {
        return !(*this < other);
    }

    // Overloading the greater than operator (>)
    bool operator>(const Vector2D<T>& other) const {
        return !(*this <= other);
    }
};

int main() {
    std::cout << "Integer vector" << std::endl;
    
    Vector2D<int> intVec1(3, 4); // Vector with length 5
    Vector2D<int> intVec2(1, 1); // Vector with length 2

    // Comparing the size of the vectors
    if (intVec1 < intVec2) {
        std::cout << "intVec1 is smaller than intVec2" << std::endl;
    } else if (intVec1 == intVec2) {
        std::cout << "intVec1 is equal to intVec2" << std::endl;
    } else {
        std::cout << "intVec1 is larger than intVec2" << std::endl;
    }
    
    std::cout << "Double vector" << std::endl;    

    Vector2D<double> doubleVec1(2.8, 1.57); // Vector with length 3.2101
    Vector2D<double> doubleVec2(1.1, 4.3);  // Vector with length 4.4385

    // Comparing the size of the vectors
    if (doubleVec1 < doubleVec2) {
        std::cout << "doubleVec1 is smaller than doubleVec2" << std::endl;
    } else if (doubleVec1 == doubleVec2) {
        std::cout << "doubleVec1 is equal to doubleVec2" << std::endl;
    } else {
        std::cout << "doubleVec1 is larger than doubleVec2" << std::endl;
    }

    return 0;
}

```
{: file='Template example'}