---
title: Data structures in c++
author: chulmin
date: 2024-06-15 00:00:00 -0500
categories: [c++, Data structure and algorithm]
tags: [Data structure, vector, stack, queue, set, array, map, c++]
math: true
---

## Data structures in c++
Unlike the C language, C++ provides various data structures in its standard library. Understanding the available data structures and their behaviors is crucial for becoming a proficient developer. This post reviews the data structures provided by the Standard Template Library and their time complexity.

## Array
std::array is a container provided by the C++ Standard Library that represents a fixed-size array. Since std::array has a fixed size known at compile-time, its operations have predictable and efficient time complexities. 


| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Access (by index)            | arr[1]       |   $$ O(1) $$  |
| Access (bounds-checked)      | arr.at(1)    |   $$ O(1) $$  |
| Front                        | arr.front()  |   $$ O(1) $$  |
| Back                         | arr.back()   |   $$ O(1) $$  |
| Fill                         | arr.fill(10) |   $$ O(n) $$  |


## Vector
std::vector is one of the most commonly used containers in the C++ Standard Library due to its dynamic size and efficient access


| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Access (by index)            | vec[index]       |   $$ O(1) $$  |
| Access (bounds-checked)      | vec.at(index)    |   $$ O(1) $$  |
| Front                        | vec.front()  |   $$ O(1) $$  |
| Back                         | vec.back()   |   $$ O(1) $$  |
| Insertion                    | vec.push_back() |  $$ O(1) $$, $$ O(n) $$ in worst case  |
| Insertion                    | vec.insert()    |  $$ O(n) $$  |
| Deletion                     | vec.pop_back()  |  $$ O(1) $$  |
| Deletion                     | vec.erase()     |  $$ O(n) $$  |
| Resizing                     | vec.resize()    |  $$ O(n) $$  |


- The time complexity of insert() is $$ O(n) $$ in the worst case, as it may require shifting elements and potentially resizing the vector.

- The time complexity of erase() is $$ O(n) $$ due to the need to shift subsequent elements to fill the gap.

## Stack, Queue, and Deque
A stack is a Last-In-First-Out (LIFO) data structure. This means that the last element added to the stack will be the first one to be removed. 


| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Access                       | stack.top()      |   $$ O(1) $$  |
| Enqueue                     | stack.push()     |   $$ O(1) $$  |
| Dequeue                        | stack.pop()      |   $$ O(1) $$  |

A queue is a First-In-First-Out (FIFO) data structure. This means that the first element added to the queue will be the first one to be removed. 

| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Access                       | queue.back()      |   $$ O(1) $$  |
| Enqueue                      | queue.push()     |   $$ O(1) $$  |
| Dequeue                        | queue.pop()      |   $$ O(1) $$  |

A deque (pronounced "deck") stands for double-ended queue. It is a flexible data structure that allows insertion and deletion of elements from both ends—front and back. 

| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Access                       | deque.front()      |   $$ O(1) $$  |
| Access                       | deque.back()       |   $$ O(1) $$  |
| Access                       | deque[index]       |   $$ O(1) $$  |
| Access                       | deque.at(index)       |   $$ O(1) $$  |
| Enqueue                     | deque.push_front() |   $$ O(1) $$  |
| Enqueue                     | deque.push_back()  |   $$ O(1) $$  |
| Insertion                    | deque.insert()     |   $$ O(n) $$  |
| Dequeue                        | deque.pop_front()  |   $$ O(1) $$  |
| Dequeue                        | deque.pop_back()   |   $$ O(1) $$  |
| Remove                       | deque.erase()      |   $$ O(n) $$  |
| Access                       | deque.front()      |   $$ O(1) $$  |
| Access                       | deque.back()       |   $$ O(1) $$  |

- The time complexity of insert() and erase() are O(n) since they might require shifting elements.

- Q. Why might one choose to use a stack or queue instead of a deque?

    While std::deque can technically fulfill the roles of both std::stack and std::queue, using std::stack or std::queue provides **clearer intent**, **better encapsulation** of behavior, and potentially more **efficient implementations** tailored to specific stack or queue operations. Therefore, you would choose std::stack or std::queue over std::deque when **clarity**, **interface specialization**, and **potentially optimized performance** for stack or queue operations are desired.

## Set and map
A set is a powerful container that stores unique elements in a specific order. Sets are particularly useful when you need to manage collections of distinct items and perform operations such as search, insertion, and deletion efficiently.

| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Insertion                    | set.insert()      |   $$ O(log(n)) $$  |
| Deletion                     | set.erase()       |   $$ O(log(n)) $$  |
| Search                       | set.find()         |   $$ O(log(n)) $$  |
| Search                       | set.count()        |   $$ O(log(n)) $$  |
| Access                       | set[index]  |   $$ O(log(n)) $$  |
| Access                       | set.at(index)  |   $$ O(log(n)) $$  |

A map is a powerful container that stores elements in key-value pairs. It is implemented as a balanced binary search tree, typically a Red-Black Tree, which allows for efficient searching, insertion, and deletion operations.

| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Insertion                    | map.insert()      |   $$ O(log(n)) $$  |
| Insertion                    | map[index] = value  |   $$ O(log(n)) $$  |
| Deletion                     | map.erase()       |   $$ O(log(n)) $$  |
| Search                       | map.find()         |   $$ O(log(n)) $$  |
| Search                       | map.count()        |   $$ O(log(n)) $$  |
| Access                       | map[index]  |   $$ O(log(n)) $$  |
| Access                       | map.at(index)  |   $$ O(log(n)) $$  |


- The time complexity of std::set and std::map is $$ O(log(n)) $$ for operations like insertion and search. This efficiency arises from their utilization of balanced binary search trees. During insertion, the algorithm locates the appropriate position among existing elements, ensuring the tree's balanced structure. Likewise, accessing elements within these containers involves the binary search algorithm to swiftly pinpoint items based on their keys."

## List
A list is a container hat implements a doubly linked list. This container is particularly useful when frequent insertions and deletions at arbitrary positions are required, as these operations are efficient in a linked list.

| Operation                    | Usage         | Time complexity |
| :--------------------------- | :--------------- | ------: |
| Insertion                    | list.insert()       |  $$ O(1) $$  |
| Insertion                    | list.push_front()    |  $$ O(1) $$  |
| Insertion                    | list.push_back()    |  $$ O(1) $$  |
| Deletion                     | list.erase()         |  $$ O(1) $$  |
| Deletion                     | list.pop_front()  |  $$ O(1) $$  |
| Deletion                     | list.pop_back()  |  $$ O(1) $$  |
| Access                       |      |  $$ O(n) $$  |


- A list is implemented as a doubly linked list, meaning each node in the list contains a data element and pointers to both the next and previous nodes.

- Accessing an element by position (index) requires traversal from the beginning or end of the list.
This takes linear time $$ O(n) $$ as there is no direct indexing.

## Usage of an std::array
```cpp
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    // Using begin() and end()
    for (auto it = arr.begin(); it != arr.end(); ++it) {
        std::cout << *it << " "; 
    }
    std::cout << std::endl; // 1 2 3 4 5 

    // Using rbegin() and rend()
    for (auto it = arr.rbegin(); it != arr.rend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl; // 5 4 3 2 1

    // Using cbegin() and cend()
    for (auto it = arr.cbegin(); it != arr.cend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl; // 1 2 3 4 5 

    // Using crbegin() and crend()
    for (auto it = arr.crbegin(); it != arr.crend(); ++it) {
        std::cout << *it << " ";
    }
    std::cout << std::endl; // 5 4 3 2 1

    return 0;
}

```
{: file='Member1'}


```cpp
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> arr = {1, 2, 3};

    // Using size()
    std::cout << "Size of array: " << arr.size() << std::endl; // 3

    // Using empty()
    std::cout << "Is array empty: " << (arr.empty() ? "true" : "false") << std::endl; // false

    return 0;
}

```
{: file='Member2'}

```cpp

#include <iostream>
#include <array>

int main() {
    std::array<int, 5> arr = {1, 2, 3, 4, 5};

    // Using operator[]
    std::cout << "Element at index 2: " << arr[2] << std::endl; // 2

    // Using at()
    std::cout << "Element at index 3: " << arr.at(3) << std::endl; // 3

    // Using front()
    std::cout << "First element: " << arr.front() << std::endl; // 1

    // Using back()
    std::cout << "Last element: " << arr.back() << std::endl; // 5

    // Using data()
    int* ptr = arr.data();
    std::cout << "First element using data(): " << *ptr << std::endl; // 1

    return 0;
}

```
{: file='Member3'}


```cpp
#include <iostream>
#include <array>

int main() {
    std::array<int, 5> arr1 = {1, 2, 3, 4, 5};
    std::array<int, 5> arr2 = {6, 7, 8, 9, 10};

    // fill(value)
    arr1.fill(100); 
    for (int i : arr1) {
        std::cout << i << " ";
    }
    std::cout << std::endl; // 100 100 100 100 100

    // swap(arr)
    arr1.swap(arr2); 
    std::cout << "arr1 after swap: ";
    for (int i : arr1) {
        std::cout << i << " ";
    }
    std::cout << std::endl; // 6 7 8 9 10

    std::cout << "arr2 after swap: ";
    for (int i : arr2) {
        std::cout << i << " ";
    }
    std::cout << std::endl; // 100 100 100 100 100

    return 0;
}


```
{: file='Member4'}


