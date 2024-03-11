---
title: How to integral in the computer world
author: chulmin
date: 2024-03-10 00:00:00 -0500
categories: [Numerical analysis]
tags: [Numerical analysis, Numerical integration, simpson, trapezoid, gauss point, Gauss integral, Gauss, integral]
math: true
---


The integral, a fundamental mathematical concept, serves to compute the area under a curve. By evaluating the integral of a function across a specified interval, we ascertain the area enclosed by the curve and the x-axis within that range. This concept finds application across diverse fields including physics, engineering, and economics.

Let us reflect on how the integral is derived. One method commonly employed is the Riemann sum, which serves to approximate the area under a curve or the definite integral of a function. The basic premise of the Riemann sum involves dividing the interval of interest into smaller subintervals and approximating the area under the curve within each subinterval. This is achieved by constructing rectangles or other geometric shapes that collectively represent the area. Taking the limit as the size of the subintervals approaches zero yields the integral.

$$
\begin{equation}
  \int_{x_0}^{x_n} f(x) dx = \lim_{k\rightarrow\infty} \sum_{k=0}^{n-1} (f(x_k)+f(x_{k+1}))(x_{k+1} - x_{k})/2
  \label{eq:Integral}
\end{equation}
$$

## Trapizoid rule 
In the computer world, where the concept of limit is not applicable, we substitute the limit with a for loop with a sufficiently large number of iterations. For instance, if we have a function $f(x)$ and wish to divide the desired area into trapezoids, then:

$$
\begin{equation}
   \int_{x_0}^{x_n} f(x) dx \approx \sum_{k=0}^{n-1} (f(x_k)+f(x_{k+1}))(x_{k+1} - x_{k})/2
  \label{eq:RiemannSum}
\end{equation}
$$

This is known as the trapezoidal rule. In this method, the function $f(x)$ is approximated as a linear function within each small interval.

## Mid point rule and Simpson's rule
Similarly, if the function is approximated as a constant function, it is known as the midpoint rule. 

$$
\begin{equation}
   \sum_{k=1}^n (f(\dfrac{x_k+x_{k+1}}{2}))(x_{k+1} - x_{k})/2
  \label{eq:MidPointRule}
\end{equation}
$$

Additionally, when the function is approximated as a second-order polynomial, it is referred to as Simpson's rule.

$$
\begin{equation}
   \sum_{k=1}^n (Af(x_k)+Bf(\dfrac{x_k+x_{k+1}}{2})+Cf(x_{k+1}))
  \label{eq:SimpsonRule}
\end{equation}
$$

One notes that  when formulating the rule, the interval is typically defined between \[-1,1\]. In Simpson's rule, the corresponding coefficients $A$, $B$, and $C$ are set as 1/3, 4/3, 1/3 respectively. Notably, in Simpson's rule, the midpoint is four times more influential than the boundaries. 

To determine these coefficients, one can establish three equations using the basis of each order: $f(x)=1$, $f(x)=x$, and $f(x)=x^2$. The sum of the coefficents should always be 2, as the interval length is 2.

## Gauss quadrature

The renowned mathematician Gauss also made significant contributions to numerical integration. His method begins with a special group of polynomials known as Legendre polynomials. The first few Legendre polynomials are as follows:

$$
\begin{equation}
  \displaylines{q_2(x)= \dfrac{1}{2}(3x^2-1)\\ q_3(x)= \dfrac{1}{2}(5x^3-3x)\\q_4(x)= \dfrac{1}{8}(35x^4-30x^2+3)}
  \label{eq:LegendrePolynomial}
\end{equation}
$$

The Legendre polynomials is that the integral of the product of the $n$th order Legendre polynomial and $n-1$th polynomial over $(-1,1)$ is always zero. 

$$
\begin{equation}
   \int_{-1}^{1} q_n(x)p_{n-1}(x) = 0 dx
  \label{eq:Characteristics}
\end{equation}
$$

This is also referred to as orthogonality. Leveraging this property, Gauss devised a straightforward and efficient quadrature rule. Suppose we have a polynomial function $f(x)$ of order $2n-1$, and divide this function by Legendre polynomial of order $n$ then,

$$
\begin{equation}
   f(x) = q(x)w(x)+r(x)
  \label{eq:DividedByLegendrePolynomial}
\end{equation}
$$


Now, integrating both sides of the equation, the quotient term (whose order is $n-1$) goes to zero, leaving us to work with the remainder (whose order is also $n-1$). Unlike the trapezoidal rule and Simpson's rule, the integration points are the solutions of the Legendre equation. Additionally, the corresponding weights can be calculated similarly to before.


## Example
To utilize these numerical methods for integration, you'll need to follow these steps:

1. Determine the number of divisions for the range.
2. Calculate the mapping from the standard range (-1, 1) to the divided interval using the Jacobian.
3. Compute the integration.

The given function is $\sin{(x)}x^2 + \log{(x+1)}x^3$, and the integral interval is $[0,\pi]$ for the example problem. The analytical solution is $36.1684$. The three integration methods are used for different number of intervals.

<p align="center">
<iframe src="https://drive.google.com/file/d/105nBDnmz5u2iLgSNMeDTUpd0mbIeAU3V/preview" width="299" height="240"></iframe>
  <br>
  <em>Figure 1. Error convergence</em>
</p>

```python
import numpy as np

def function(x):
  return np.sin(x) * x**2+ np.log(x+1.)*x**3

def TrapezoidRule(start = -1, end = 1, Ninterval = 1 ):
  dx = (end - start)/Ninterval
  Point  = [-1., 1.]
  Weight = [1., 1.]
  Value = 0
  for jj in range(Ninterval):
    LocalStart = start + dx * jj
    LocalEnd   = start + dx * (jj+1)
    LocalPoint = [LocalStart, LocalEnd]
    Length     = LocalEnd - LocalStart
    for ii in range(len(Point)):
      Value += function(LocalPoint[ii]) * Weight[ii] * Length * 0.5
      # Length * 0.5 <- Jacobian

  return Value

def SimpsonRule(start = -1, end = 1, Ninterval = 1 ):
  dx = (end - start)/Ninterval
  Point  = [-1., 0., 1.]
  Weight = [1./3., 4./3., 1./3.]
  Value = 0
  for jj in range(Ninterval):
    LocalStart = start + dx * jj
    LocalEnd   = start + dx * (jj+1)
    LocalPoint = [LocalStart, (LocalStart + LocalEnd)*0.5, LocalEnd]
    Length     = LocalEnd - LocalStart
    for ii in range(len(Point)):
      Value += function(LocalPoint[ii]) * Weight[ii] * Length * 0.5
      # Length * 0.5 <- Jacobian

  return Value

def GaussQuadrature(start = -1, end = 1, Ninterval = 1 ):
  dx = (end - start)/Ninterval
  GaussPoint  = [-np.sqrt(1./3.), np.sqrt(1./3.)]
  GaussWeight = [1., 1.]
  Value = 0

  for jj in range(Ninterval):
    LocalStart = start + dx * jj
    LocalEnd   = start + dx * (jj+1)
    Length     = LocalEnd - LocalStart
    LocalGaussPoint = np.array(GaussPoint) * (LocalEnd - LocalStart)*0.5 + (LocalEnd + LocalStart)*0.5
    for ii in range(len(GaussPoint)):
      Value += function(LocalGaussPoint[ii]) * GaussWeight[ii] * Length * 0.5
      # Length * 0.5 <- Jacobian

  return Value

ResultTrapezoid = []
ResultSimpson   = []
ResultGauss     = []

for kk in range(5):
  ResultTrapezoid.append( TrapezoidRule( 0, np.pi, 2**kk) )
  ResultSimpson.append( SimpsonRule( 0, np.pi, 2**kk) )
  ResultGauss.append( GaussQuadrature( 0, np.pi, 2**kk) )
```
{: file='Numerical integration'}




## Remark
1. As the saying goes, "Lazy humans tire out computers." Despite the increasing computing power, programmers must remain smart to ensure the computer's well-being.

2. How can we guarantee the accuracy of numerical integration? One method is to calculate the error between the actual area and the calculated area. Take the trapezoid rule, for instance: the error represents the area between the curve and the linear line. By reducing the interval size, we can observe this error approaching zero. However, to keep things concise, specific verification is omitted in this post.


## Reference 
[https://en.wikipedia.org/wiki/Gaussian_quadrature](https://en.wikipedia.org/wiki/Gaussian_quadrature)

Cheney, W. and Kincaid, D., 1998. Numerical mathematics and computing. International Thomson Publishing.




