---
title: Root finidng method
author: chulmin
date: 2024-04-12 00:00:00 -0500
categories: [Numerical computing]
tags: [Numerical analysis, Bisection method, Newton method]
math: true
---

Finding the roots of a function is crucial in various fields of science, engineering, and mathematics. 

## Bisection method
The bisection method is a simple and robust algorithm used to find the roots of a function. It starts by requiring two initial points, (x_0) and (x_1), and then iteratively narrows down the interval containing the root. It is essential that the function values at the initial points, (f(x_0)) and (f(x_1)), have different signs to ensure the existence of solutions for a continuous function. Then, the algorithm evaluates the sign of the function value at the midpoint, (f((x_0+x_1)/2)), and selects the new (x_0) or (x_1) with differing signs. This iterative process continues until the interval becomes sufficiently small, leading to the identification of the root. The following equation is the example function for bisection method.

$$ 
\begin{equation}
  f(x) = (x+2)(x-3)e^x
  \label{eq:Example function}
\end{equation}
$$

<br>
<p align="center">
<iframe src="https://drive.google.com/file/d/1FCMExF9kVQliAxWWGpqgg9ab9vaXMH2-/preview" width=" 350.4" height="281" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Given function and two initial points for the bisection method</em>
</p>


```python
import numpy as np

def function(x):
  return (x+2.) * (x-3.) *  np.exp(x)

def bisection_method(func, x1, x2, tol=1e-6, max_iter=100):
    if func(x1) * func(x2) >= 0:
        raise ValueError("The function values at x1 and x2 must have opposite signs.")
    
    iter_count = 0
    while iter_count < max_iter:
        x_mid = (x1 + x2) / 2
        f_mid = func(x_mid)
        
        if np.abs(f_mid) < tol:
            return x_mid, iter_count
        
        if func(x1) * f_mid < 0:
            x2 = x_mid
        else:
            x1 = x_mid
        
        iter_count += 1
        print( x_mid )
    
    raise ValueError("Maximum number of iterations reached without convergence.")

# Initial guess values
x1 = 2.2
x2 = 3.3

root, iterations = bisection_method(function, x1, x2)
print("Root found:", root)
print("Iterations:", iterations)
```
{: file='Bisection method'}


| iteration (n) | $(x_n + x_{n+1})/2$ | 
| :---------| :----------------|  
| 1	| 2.75 | 
| 2	| 3.025 | 
| 3	| 2.8875 | 
| 4	| 2.95625 | 
| 5	| 2.990625 | 
| 6	| 3.0078125 | 
| 7	| 2.99921875 | 
| 8	| 3.003515625 | 
| 9	| 3.001367187 | 
| 10 | 	3.000292969 | 
| 11 | 	2.999755859 | 
| 12 | 	3.000024414 | 
| 13 | 	2.999890137 | 
| 14 | 	2.999957275 | 
| 15 | 	2.999990845 | 
| 16 | 	3.000007629 | 
| 17 | 	2.999999237 | 
| 18 | 	3.000003433 | 
| 19 | 	3.000001335 | 
| 20 | 	3.000000286 | 
| 21 | 	2.999999762 | 
| 22 | 	3.000000024 | 
| 23 | 	2.999999893 | 
| 24 | 	2.999999958 | 

## Newton method
Newton method is also an iterative algorithm for finding the root. Unlike the bisection method, it commences with one initial point and approaches to the solution by utilizing the derivative of the function. Starting with the initial guess, a linear line is drawn, and the root of this line is found, which serves as the next point in the iteration.


<br>
<p align="center">
<iframe src="https://drive.google.com/file/d/1EpbVRC_eIGRcCBIaoGGuzCh_LOkEo7mt/preview" width=" 350.4" height="279" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Bits allocation for single-precision floating-point numbers</em>
</p>

$$ 
\begin{equation}
  l(x) = f'(x_n)(x-x_n) + f(x_n) \\
  x_{n+1} = x_n - \dfrac{ f(x_n) }{ f'(x_n) } 
  \label{eq:Newton method}
\end{equation}
$$


```python
import numpy as np

def function(x):
    return (x + 2) * (x - 3) * np.exp(x)

def derivative(x):
    return (2 * x - 1) * np.exp(x)

def newton_method(func, derivative, x0, tol=1e-6, max_iter=100):
    x = x0
    iter_count = 0
    
    while iter_count < max_iter:
        f_x = func(x)
        f_prime_x = derivative(x)
        
        if np.abs(f_prime_x) < tol:
            raise ValueError("Derivative close to zero. Newton's method failed.")
        
        x_new = x - f_x / f_prime_x
        
        if np.abs(x_new - x) < tol:
            return x_new, iter_count
        
        x = x_new
        iter_count += 1
        print( x )
    
    raise ValueError("Maximum number of iterations reached without convergence.")

# Initial guess value
x0 = 1.5

root, iterations = newton_method(function, derivative, x0)
print("Root found:", root)
print("Iterations:", iterations)

```
{: file='Newton method'}


This method shows the more efficient performance compared to the bisection method.

| iteration (n) | $x_n$ | 
| :---------| :----------------|  
| 1	| 4.12500000000000 | 
| 2	| 3.17456896600000 | 
| 3	| 3.00569705300000 | 
| 4	| 3.00000647700000 | 
| 5	| 3.000000000008389 | 

However, the selection of an appropriate initial point is crucial, as an unsuitable choice may lead to divergence.

<br>
<p align="center">
<iframe src="https://drive.google.com/file/d/1FQiO_bkncdo9IsySqG29QBmQtIDX1QM1/preview" width=" 349" height="280" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. Divergence due to the inappropriate starting point</em>
</p>

## Newton method for the system equation
The Newton's method can be extended to solve systems of equations. For instance, consider the following system of equations which can be solved using the Newton's method, yielding roots at $x=0$, $y=2$, and $z=3$:

$$ 
\begin{equation}
  \begin{aligned}
    & x+y+z = 5 \\
    & x^2+y^2+z^2 = 13 \\
    & e^x+xy+xz = 1 \\								
  \end{aligned}	
  \label{eq:Newton method for the system equation}
\end{equation}
$$

 

```python 
import numpy as np

x0 = np.array([1., 3., 5.])

def function(x):
    f = np.array([x[0] + x[1] + x[2] - 5.,
                  x[0]**2 + x[1]**2 + x[2]**2 - 13.,
                  np.exp(x[0]) + x[0]*x[1] - x[0]*x[2] - 1.])
    return f

def derivative(x):
    df = np.array([[1., 1., 1.],
                   [2.*x[0], 2.*x[1], 2.*x[2]],
                   [np.exp(x[0]) + x[1] - x[2], x[0], -x[0]]])
    return df

def newton_method_system(func, derivative, x0, tol=1e-6, max_iter=100):
    x = x0
    iter_count = 0
    
    while iter_count < max_iter:
        f_x = func(x)
        jacobian = derivative(x)
        
        if np.linalg.norm(f_x) < tol:
            print(np.linalg.norm(f_x))
            return x, iter_count
        
        try:
            x_new = x - np.linalg.solve(jacobian, f_x)
        except np.linalg.LinAlgError:
            raise ValueError("Singular Jacobian matrix. Newton's method failed.")
        
        print(np.linalg.norm(f_x))
        x = x_new
        iter_count += 1
    
    raise ValueError("Maximum number of iterations reached without convergence.")

# Initial guess value
x0 = np.array([1., 3., 5.])

root, iterations = newton_method_system(function, derivative, x0)
print("Root found:", root)
print("Iterations:", iterations)

```
{: file='Newton method for system'}


| iteration (n) | $ \|\|f(x_n)\|\|_2 $ | 
| :---------| :----------------|  
| 1 |  22.3624543627970 | 
| 2 |  8.69354732514425 | 
| 3 |  1.48209143410228 | 
| 4 |  0.126598379841041 | 
| 5 |  0.0212901707189886 | 
| 6 |  0.00609083316951065 | 
| 7 |  0.00149434368995687 | 
| 8 |  0.000374527505121139 | 
| 9 |  9.36714099866895e-05 | 
| 10 |  2.34237308180405e-05 | 
| 11 |  5.85667658839108e-06 | 
| 12 |  1.46426296802727e-06 | 
| 13 |  3.66077521047241e-07 | 

## Remark
When dealing with scalar values, determining the error involves a straightforward subtraction of the real value from the approximated value. If only the magnitude of the error is required, the sign can be disregarded. However, when working with multidimensional data such as 2D vectors or 3D matrices, the concept of "norm" comes into play. In numerical analysis, the p-norm is often employed as a common method for handling high-dimensional values.

$$ 
\begin{equation}
|| \boldsymbol {x} ||_p = \left( \sum_{i=1}^{n} |x_i|^p \right)^{1/p}
  \label{eq:p norm}
\end{equation}
$$



## Reference
[Wikipedia-Machine epsilon](https://en.wikipedia.org/wiki/Machine_epsilon)<br>
[Wikipedia-Single-precision floating-point format](https://en.wikipedia.org/wiki/Single-precision_floating-point_format)<br>


