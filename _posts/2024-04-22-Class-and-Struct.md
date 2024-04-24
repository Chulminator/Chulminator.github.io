---
title: Class and Struct
author: chulmin
date: 2024-04-22 00:00:00 -0500
categories: [c++]
tags: [Class, Struct]
math: true
---

Struct is a concept that already exists in the C language, whereas class is a newly adopted concept in C++. Additionally, in C++, additional functionalities have been introduced to structs.

## Struct 
Structures are essential for organizing multiple pieces of data together. 

```cpp
struct Human {
    std::string name;
    int age;
};

int main() {
    // create struct variable
    Human human1;

    // Assign values
    human1.name = "Chulminator";
    human1.age = 16;

    // create and assign at the same time
    Human human2 = {"Chulmander", 20};    
    return 0;
}

```
{: file='struct declare'}


```cpp
struct Elf : public Human {
    double earSize;
};


int main() {
    // create struct variable
    Elf elf1;
    elf1.name    = "Regolas";
    elf1.age     = 1020;
    elf1.earSize = 10;    
    return 0;
}
```
{: file='struct overiding'}

Alternatively, constructors (initialization lists) can be defined:

```cpp

struct Human {
    Human(std::string n, int a) : 
    name(n), 
    age(a) 
    {
        std::cout << "Human object is created." << std::endl;
    }
    std::string name;
    int age;
};

int main() {
    // create and assign at the same time
    Human person("Alice", 25);
    std::cout << "Name: " << person.name << ", Age: " << person.age << std::endl;

    return 0;
}
```
{: file='struct constructor}

### Operator
At times, it may be necessary to define operators for structs. For instance, consider a struct that stores a 2-dimensional vector. Operators enable to compare the size of the vectors using operators.


```cpp
#include <iostream>

struct Vector2D {
    int x;
    int y;

    // Constructor
    Vector2D(int x_val, int y_val):
    x(x_val),
    y(y_val)
    {

    }

    // Overloading the less than operator (<)
    bool operator<(const Vector2D& other) const {
        return (x * x + y * y) < (other.x * other.x + other.y * other.y);
    }

    // Overloading the less than or equal to operator (<=)
    bool operator<=(const Vector2D& other) const {
        return (x * x + y * y) <= (other.x * other.x + other.y * other.y);
    }

    // Overloading the equal to operator (==)
    bool operator==(const Vector2D& other) const {
        return (x == other.x) && (y == other.y);
    }

    // Overloading the not equal to operator (!=)
    bool operator!=(const Vector2D& other) const {
        return !(*this == other);
    }

    // Overloading the greater than or equal to operator (>=)
    bool operator>=(const Vector2D& other) const {
        return !(*this < other);
    }

    // Overloading the greater than operator (>)
    bool operator>(const Vector2D& other) const {
        return !(*this <= other);
    }
};

int main() {
    Vector2D vec1(3, 4); // Vector with length 5
    Vector2D vec2(1, 1); // Vector with length 2

    // Comparing the size of the vectors
    if (vec1 < vec2) {
        std::cout << "vec1 is smaller than vec2" << std::endl;
    } else if (vec1 == vec2) {
        std::cout << "vec1 is equal to vec2" << std::endl;
    } else {
        std::cout << "vec1 is larger than vec2" << std::endl;
    }

    return 0;
}
```
{: file='struct operator}

This functionality is also available in operators:

```cpp
#include <iostream>

struct PlayerLocation {
    PlayerLocation(double x = 0.0, double y = 0.0):
    location{x, y}
    {

    }

    void operator()(double velocity[2], double time) {
        location[0] += velocity[0] * time;
        location[1] += velocity[1] * time;
    }

    double location[2];
};

int main() {
    PlayerLocation player(1.0, 2.0);

    double velocity[2] = {2.0, 3.0};
    double time = 1.5;

    player(velocity, time);

    std::cout << "Player's new location: (" << player.location[0] << ", " << player.location[1] << ")" << std::endl;

    return 0;
}

```
{: file='struct operator1}


## Class 

Unlike structs, class members are private by default, accessible only by class members. Classes have three access levels: public, protected, and private. Public functions or variables are accessible from anywhere, including outside the class, within the derived class, and by class members. Protected functions or variables are accessible by the derived class and by class members, but not from outside the class. Private functions or variables are only accessible by class members and not from outside the class.





| Public | 
| Outside the class | Derived class | class member | 
| :---------| :----------------|  :----------------|
| O	| O | O | 


| Protectd | 
| Outside the class | Derived class | class member | 
| :---------| :----------------|  :----------------|
| X	| O | O | 


| Private | 
| Outside the class | Derived class | class member | 
| :---------| :----------------|  :----------------|
| X	| X | O | 

```cpp
#include <iostream>

class Base {
public:
    int publicVar;

    void publicFunction() {
        std::cout << "Public function called." << std::endl;
    }

protected:
    int protectedVar;

    void protectedFunction() {
        std::cout << "Protected function called." << std::endl;
    }

private:
    int privateVar;

    void privateFunction() {
        std::cout << "Private function called." << std::endl;
    }

public:
    void accessLevelsExample() {
        // Accessible from anywhere
        publicVar = 1;
        publicFunction();

        // Accessible from derived class and member functions
        protectedVar = 2;
        protectedFunction();

        // Accessible only from member functions
        privateVar = 3;
        privateFunction();
    }
};

class DerivedClass : public Base {
public:
    void accessBaseMembers() {
        // Can access public and protected members of the base class
        publicVar = 10;
        publicFunction();
        protectedVar = 20;
        protectedFunction();

        // Cannot access private members of the base class
        // privateVar = 30; // Error: 'int Base::privateVar' is private within this context
        // privateFunction(); // Error: 'void Base::privateFunction()' is private within this context
    }
};

int main() {
    Base obj;
    obj.accessLevelsExample();

    DerivedClass derivedObj;
    derivedObj.accessBaseMembers();

    return 0;
}


```
{: file='class operator1}

## Namespace

Namespaces are a useful tool for enhancing readability and code comprehension by structuring code. When integrating various libraries, frameworks, or modules, independently developed code may have functions, variables, or classes with the same name, leading to collisions. Using namespaces helps prevent these collisions, making code maintenance easier and preventing conflicts.


```cpp
#include <iostream>

// Define a namespace called Math
namespace Math {
    // Define a function to add two numbers
    int add(int a, int b) {
        return a + b;
    }
}

// Define another namespace called Utility
namespace Utility {
    // Define a function to print a message
    void printMessage(const std::string& message) {
        std::cout << "Message from Utility namespace: " << message << std::endl;
    }
}

int main() {
    // Call the add function from the Math namespace
    int result = Math::add(3, 5);
    std::cout << "Result of addition: " << result << std::endl;

    // Call the printMessage function from the Utility namespace
    Utility::printMessage("Hello, world!");

    return 0;
}

```
{: file='class operator1}


