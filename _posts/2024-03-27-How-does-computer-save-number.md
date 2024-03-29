---
title: How does a computer store numbers
author: chulmin
date: 2024-03-27 00:00:00 -0500
categories: [Numerical computing]
tags: [Numerical analysis, Floating number]
math: true
---

Nowadays, most universities teach Python as the first programming language. However, I started with the C language. I wasn't a great student at first. Because I encountered an obstacle from the beginning: I couldn't understand why the computer required me to define different data types such as integer, float, double, and so on. The very first chapter of numerical analysis (not the programming class) provided me with the answer.
<br>
<br>
The dual nature of fun and difficulty in numerical analysis stems from the fact that the computer world operates in a discrete manner. For example, it lacks the capacity to comprehend derivatives and integrals derived from the concept of continuity. Real numbers also pose challenges in the discrete realm. It is widely known that due to its continuous nature, we cannot propose the smallest number greater than 1.0. However, in the discrete world, it is indeed possible to identify the smallest number larger than 1.0. This difference between 1.0 and the identified number is referred to as **Machine epsilon**. To delve into the concept of **Machine epsilon**, one must first understand how computers store real numbers.


## Floating-point representation.
Normalized scientific notation is used to represent real numbers. For instance, in the normalized scientific notation, $7423.51239$ is expressed as $0.7 42351239\times 10^4 $ and $0.0231906$ is represented as $0.231906 \times 10^{-1}$. In sum, a real number $$x$$ is represented as

$$ 
\begin{equation}
  x = \pm r \times 10^n = \pm d_1 d_2 d_3 \cdots \times 10^n
  \label{eq:normalized scientific notation}
\end{equation}
$$

where $r\in \[0.1, 1\)$. $r$ is called the **normalized mantissa** and $n$ is called **exponent**.

## Discrete real number in computer world
It is somewhat ironic to combine the terms "discrete" and "real number". Because real number is a concept in the continuous world, which leads to the mantissa can accommodate an infinite number of decimal digits. However, in the discrete world, a specific number of decimal digits are defined for real numbers. Additionally, in the computer world, which operates using the binary system, a real number $x$ is represented 

$$ 
\begin{equation}
  x = \pm r \times 2^n = \pm d_1 d_2 d_3 \cdots d_k \times 2^n
  \label{eq:binary normalized scientific notation}
\end{equation}
$$

where $d_1 = 1$ to ensure $r\in \[0.5, 1\)$. The distinction between equations \eqref{eq:normalized scientific notation} and \eqref{eq:binary normalized scientific notation} lies in the range of values that (d_k) can hold. In normalized scientific notation, (d_k) can contain decimal numbers from 0 to 9, while in the binary system, (d_k) is limited to holding only 0 and 1. The specific value of (k) depends on the type of variable.

## single-precision floating-point numbers
In single-precision floating-point numbers, one of variable types, a computer allocates 32 bits to store a real number. (If you are not familiar with the concept of a bit, you can think of it as a single box that can only hold 0 or 1.) The computer utilizes the 32 bits as follows: 1 bit for the sign, 23 bits for the mantissa, and 8 bits for the exponent, as illustrated in Figure 1.
<br>
<p align="center">
<iframe src="https://drive.google.com/file/d/17IGFsI6orJkgUxVLLqhd5NVqIfe10B41/preview" width=" 318" height="269" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Bits allocation for single-precision floating-point numbers</em>
</p>

And each bit is used as follows:

$$ 
\begin{equation}
  x = (-1)^s \times 2^{c-127} \times (1.f)_{2}
  \label{eq:standard single-precision floating-point}
\end{equation}
$$

The variable $s$ represents the sign bit: when $s = 0$, the sign is positive ($+$), and when $s = 1$, the sign is negative ($-$). The variable $c$ corresponds to the 8 bits used for the exponent. When all the bits accommodate the value $0$, $c$ holds the smallest number, $$00000000_{2}$$. Conversely, if all the bits accommodate the value $1$, $c$ holds the largest number, $$11111111_{2}=255_{10}$$. To express a small number as well, $-127$ is added on the exponent, as shown in \eqref{eq:standard single-precision floating-point}. Addtionally, the smallest number and the lartest number of tne exponent are reserved to represent the special cases, $\pm 0$ and $\pm \infty$:

$$ 
\begin{equation}
  -127 < c-127 < 128
  \label{eq:exponent}
\end{equation}
$$

Lastly, $(1.f)$ corresponds to the mantissa which utilizes 23 bits. As discussed previosly, the first bit is always $1$ in the binary system, which is why it takes $(1.f)$ form rather than $(0.f)$. Therefore, the mantissa is restricted by the following inequality:

$$ 
\begin{equation}
  1 \le (1.f)_{2} \le (1.11111\:11111\:11111\:11111\:111)_2 = 2 - 2^{-23}
  \label{eq:mantissa}
\end{equation}
$$

The number in Figure 1 represents the following number in single-precision:

$$ 
\begin{equation}
  \begin{aligned}
  x &= (-1)^1 \times 1.1010001\:01000011\:10101001 \times 2^{11001111-1111111} \\
    &= -1.6484375 \times 2^{80} \\
    &= -1.992838655770990 \times 10^{24}
  \end{aligned}
  \label{eq:Figure 1}
\end{equation}
$$

## So what value is the **machine epsilon**? 
Now, time to answer the original question "what value is the **machine epsilon**?". Again, Machine epsilon is the smallest number, which makes it different from $1$ when added to $1$. For instance, consider a tiny number, $1/2^5o$. Adding this number to $1$ will not affect to the outcome, because it is out of 23 bits to express. In that sense, the **machine epsilon** in single precision will be

$$ 
\begin{equation}
  \epsilon = (0.00000\:00000\:00000\:00000\:001)_2 \times 2^0
  \label{eq:Machine epsilon}
\end{equation}
$$

Adding this number to 1 will be

$$ 
\begin{equation}
  \begin{aligned}
   1 + \epsilon = & (-1)^0 \times 1.00000\:00000\:00000\:00000\:000 \times 2^{1111111-1111111} \\  
                + & (0.00000\:00000\:00000\:00000\:001)_2 \times 2^0 \\
                = & (1.00000\:00000\:00000\:00000\:001)_2 \times 2^0 \\
  \end{aligned}
  \label{eq:Machine epsilon add 1}
\end{equation}
$$

You can check this in any laguage. Specifically in python, this will be

```python 
import numpy as np
print(np.finfo(np.float32).eps)
print(1/pow(2,23))
print(np.finfo(float).eps)
print(1/pow(2,52))
print(1/pow(2,52)+1 == 1)
print(1/pow(2,53)+1 == 1)
```
And the corresponding output is 

```bash 
1.1920929e-07
1.1920928955078125e-07
2.220446049250313e-16
2.220446049250313e-16
False
True
```
{: file='result'}

The first value represents the machine epsilon in single precision, which is equivalent to $1/2^{23}$. The second line details this value. The third value denotes the machine epsilon in double precision, which corresponds to $1/2^{52}$, as shown in the fourth line. The fifth line demonstrates that adding the machine epsilon to $1.0$ yields a result different from $1.0$. Moreover, the sixth line illustrates that adding a number smaller than the machine epsilon does not impact the result of the operation. Furthermore, it is worth noting that Python employs double precision by default.

## Reference
[Wikipedia-Machine epsilon](https://en.wikipedia.org/wiki/Machine_epsilon)<br>
[Wikipedia-Single-precision floating-point format](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)<br>


