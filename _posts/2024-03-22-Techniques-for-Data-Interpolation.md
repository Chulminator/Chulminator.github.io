---
title:  Techniques for Data Interpolation
author: chulmin
date: 2024-03-22 00:00:00 -0500
categories: [Algorithm, Numerical computing]
tags: [Numerical analysis, Lagrange Interpolation, natural cubic spline]
math: true
---


We possess temperature data recorded throughout a day, from midnight to midnight the following day. Suppose we seek to estimate the temperature at any given time between these data points. What approach would be most appropriate for estimating the temperature, let's say, at 3:00 PM A straightforward method might involve drawing a linear line between the recorded temperatures at 2:00 PM and 4:00 PM, and then determining the temperature corresponding to $x=3$. Interpolation and spline techniques offer systematic methodologies for making such estimates within the data range.

## Problem definition
Let's say the following temperature data is the temperature recored around equator on Mars. 

<p align="center">
<iframe src="https://drive.google.com/file/d/15syFBC0VrnPLhw-PVnS0Uirrv8KQId2l/preview" width="350" height="281" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Data points and validation data</em>
</p>


## Lagrange interpolation
To define a unique linear function, you need two points, and to define a unique parabola, you require three points.  Similarly, in Lagrange interpolation, a polynomial with an order one less than the number of data points is determined. This involves setting up equations to find the corresponding coefficients using the Vandermonde matrix. In this context, the code adopts the Lagrange form. In Lagrange form, for each data point $(x_i, y_i)$, constructing a polynomial $l_i(x)$ such that $l_i(x_i) = 1$ and $l_i(x_j) = 0$ where $i \neq j$. Then, interpolation polynomial is
<br>

$$
\begin{equation}
  p_n(x) = \sum_{i=1}^n l_i(x)y_i
  \label{eq:interpolation polynomial}
\end{equation}
$$


```python
import numpy as np
import matplotlib.pyplot as plt

# Temperature over time on Mars
data_time        = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24])
data_temperature = np.array([-73, -72, -70, -67, -65, -60, -55, -50, -40, -30, -20, -5, 5, 10, 15, 18, 20, 15, 10, 0, -10, -25, -40, -57, -73])

# Lagrange interpolation - lagrange form
def lagrange_interpolation(x, y, x_new):
    total = 0
    n = len(x)
    for i in range(n):
        xi, yi = x[i], y[i]
        pi = 1
        for j in range(n):
            if i != j:
                pi *= (x_new - x[j]) / (xi - x[j])
        total += yi * pi
    return total

# Calculating for new x
x_new = np.linspace(min(data_time), max(data_time), 100)
y_new = [lagrange_interpolation(data_time[::2], data_temperature[::2], xi) for xi in x_new]
```
{: file='Lagrange interpolation'}

<p align="center">
<iframe src="https://drive.google.com/file/d/15kbUBNEwPkap1oOSmESEpcB0LMpa8Z0s/preview" width="350" height="281" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. Result: Lagrange interpolation </em>
</p>


## Natural cubic spline
Lagrange interpolation uses a high-order polynomial, while natural cubic spline uses a cubic polynomial for each interval. 

$$
\begin{equation}  
  S(x) = \begin{cases}
    S_0(x) & (x_0 \leq x_1)\\
    S_1(x) & (x_1 \leq x_2)\\
    \,\,\vdots  \\ 
    S_{n-1}(x) & (x_{n-1} \leq x_n)
  \end{cases}
  \label{eq:Spline function}
\end{equation}
$$

It requires 4 pieces of information to define a unique cubic polynomial. Then given $n$ intervals, $4n$ conditions are required. For each interval, the polynomial should go through corresponding two points, which yields to $2n$. 

$$
\begin{equation}  
\begin{cases}
  S_i(x_i) = y_i \\  
  S_i(x_{i+1}) = y_{i+1} \\
\end{cases}
\end{equation}
  \label{eq:condition1}
$$

And continuity condition is imposed for the first and second derivatives at the $n-1$ interior points, which give $2(n-1)$. 

$$
\begin{equation} 
\begin{cases} 
  S_i'(x_{i+1}) = S_{i+1}'(x_{i})\\
  S_i''(x_{i+1}) = S_{i+1}''(x_{i})
\end{cases}
  
\end{equation}
\label{eq:condition2}
$$

Now we realize that we need two more conditions, which we typically impose at the start and end.

$$
\begin{equation} 
  S''(x_{0}) = S''(x_{n})  =0
\end{equation}
\label{eq:condition3}
$$

To implement this, we set up a system equation based on the second derivative information. With $z_i \equiv S'' (x_i) $,

$$
\begin{equation} 
  S''_i(x) = \dfrac{z_{i+1}}{h_i}(x-x_i) + \dfrac{z_{1}}{h_i}(x_{i+1}-x)
\end{equation}
\label{eq:SecondDerivateS}
$$

Then,

$$
\begin{equation} 
  S_i(x) = \dfrac{z_{i+1}}{6h_i}(x-x_i)^3 + \dfrac{z_{1}}{6h_i}(x_{i+1}-x)^3 + C_i(x- x_i) + D_i( x_{i+1}-x)
\end{equation}
\label{eq:S}
$$

$C_i$ and $D_i$ can be calculated using $C_0$ continuity:

$$
\begin{equation} 
\begin{cases} 
  C_i = ( \dfrac{y_{i+1}}{h_i} - \dfrac{h_{i}}{6}z_{i+1} )\\
  D_i = ( \dfrac{y_{i}}{h_i} - \dfrac{h_{i}}{6}z_{i} )\\
\end{cases}
\end{equation}
\label{eq:CiDi}
$$

Impose $C_1$ continuity on this set of equations:

$$
\begin{equation} 
\begin{cases} 
  S'_i(x_i) = -\dfrac{ h_{i} }{ 6 } z_{i+1} - \dfrac{ h_{i} }{ 3 } z_i + b_i \\ 
  S'_{i-1}(x_i) = \dfrac{ h_{i-1} }{ 6 } z_{i-1} + \dfrac{ h_{i-1} }{ 3 } z_i + b_{i-1}
\end{cases}
\end{equation}
\label{eq:C1continuity}
$$

, where $b_i = \dfrac{1}{h_i}(y_{i+1} - y_i)$. we obtain a system of equations:

$$
\begin{equation} 
\begin{cases} 
    z_0 = 0 \\
    h_{i-1} z_{i-1} + 2(h_{i-1} + h_i )z_i + h_i z_{i+1} = 6(b_i - b{i-1}) \\
    z_n = 0
\end{cases}
\end{equation}
\label{eq:System equation}
$$

Natural cubic spline can be caluclated by solving the system equations.

```python
## Temperature over time on Mars
data_time = np.array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24])
data_temperature = np.array([-73, -72, -70, -67, -65, -60, -55, -50, -40, -30, -20, -5, 5, 10, 15, 18, 20, 15, 10, 0, -10, -25, -40, -57, -73])

def natural_cubic_spline(x, y):
    n = len(x) - 1
    h = [x[i+1] - x[i] for i in range(n)]

    #Step 1: Create the tridiagonal matrix algorithm
    A = np.zeros((n+1, n+1))
    A[0,0] =1
    A[n,n] =1
    for i in range(1, n):
        A[i, i-1] = h[i-1]
        A[i, i] = 2 * (h[i-1] + h[i])
        A[i, i+1] = h[i]

    #Step 2: Calculate the right-hand side of the equation
    alpha = [(3/h[i]) * (y[i+1] - y[i]) - (3/h[i-1]) * (y[i] - y[i-1]) for i in range(1, n)]
    B = np.array([0] + alpha + [0])

    #Step 3: Solve the system of equations using Thomas algorithm
    c = np.zeros(n+1)
    c = np.linalg.solve(A, B)

    #Calculate the remaining coefficients
    b = [(y[i+1] - y[i])/h[i] - h[i] * (c[i+1] + 2*c[i])/3 for i in range(n)]
    d = [(c[i+1] - c[i]) / (3 * h[i]) for i in range(n)]

    return b, c, d

# Calculate coefficients for the natural cubic spline
b, c, d = natural_cubic_spline(data_time[::2], data_temperature[::2])

# Evaluate the spline at new x values
def evaluate_spline(x, y, b, c, d, x_new):
    y_new = []
    for x_val in x_new:
        idx = np.where(x <= x_val)[0][-1]
        h = x_val - x[idx]
        y_val = y[idx] + b[idx] * h + c[idx] * h**2 + d[idx] * h**3
        y_new.append(y_val)
    return y_new
```
{: file='Natural cubic spline'}

<p align="center">
<iframe src="https://drive.google.com/file/d/15oCjDzAfm1o_qaS8TI5Wcxhpmns9V8e_/preview" width="350" height="281" allow="autoplay"></iframe>
  <br>
  <em>Figure 3. Result: Natural cubic spline</em>
</p>

## Multi-layer perceptron (neural network)

The two methods mentioned above are tailored for one-dimensional datasets, whereas machine learning techniques have proven effective in higher dimensions. These algorithms utilize multi-layer perceptron structures to model complicated functions and derive the associated coefficients through optimization algorithms. However, validating this approach poses challenges when applied to complex data and is susceptible to overfitting. In such cases, although the neural network can be accurate on the provided data, it may struggle to infer meaningful logic between the data points.

```python
import torch
import torch.nn as nn
import torch.optim as optim
import matplotlib.pyplot as plt

# DataSet
data_time = torch.tensor([0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24], dtype=torch.float32)
data_temperature = torch.tensor([-73, -72, -70, -67, -65, -60, -55, -50, -40, -30, -20, -5, 5, 10, 15, 18, 20, 15, 10, 0, -10, -25, -40, -57, -73], dtype=torch.float32)


# Normalization
data_time_normalized = (data_time - data_time.min()) / (data_time.max() - data_time.min())
data_temperature_normalized = (data_temperature - data_temperature.min()) / (data_temperature.max() - data_temperature.min())

# MLP
class MLPModel(nn.Module):
    def __init__(self):
        super(MLPModel, self).__init__()
        self.fc1 = nn.Linear(1, 16)
        self.fc2 = nn.Linear(16, 16)
        self.fc3 = nn.Linear(16, 1)
        self.Act = nn.Tanh()

    def forward(self, x):
        x = self.Act(self.fc1(x))
        x = self.Act(self.fc2(x))
        x = self.fc3(x)
        return x

# loss function and optimization algorithm
model = MLPModel()
criterion = nn.MSELoss()
optimizer = optim.NAdam(model.parameters(), lr=0.01)

# training
epochs = 10000
for epoch in range(epochs):
    optimizer.zero_grad()
    outputs = model(data_time_normalized[0::2].unsqueeze(1))
    loss = criterion(outputs, data_temperature_normalized[0::2].unsqueeze(1))
    loss.backward()
    optimizer.step()
    if epoch % 1000 == 0:
        print(f'Epoch [{epoch+1}/{epochs}], Loss: {loss.item():e}')

# visualization
x_new = torch.linspace(0,1,100, dtype=torch.float32)
x_new_recoverd = x_new*(data_time.max() - data_time.min()) + data_time.min()
y_new = model(x_new.unsqueeze(1)).detach()
y_new_recoverd = y_new*(data_temperature.max() - data_temperature.min()) + data_temperature.min()
plt.plot(data_time[0::2].numpy(), data_temperature[0::2].numpy(), 'bo', label='Original data')
plt.plot(data_time[1::2].numpy(), data_temperature[1::2].numpy(), 'rs', label='Validation')
plt.plot(x_new_recoverd.numpy(), y_new_recoverd.numpy(), label='MLPFitted line')
plt.legend()
plt.show()
```
{: file='MLP-pytorch'}

Above code yields to

```bash
Epoch [1/10000], Loss: 1.916926e-01
Epoch [1001/10000], Loss: 8.592647e-04
Epoch [2001/10000], Loss: 2.964659e-04
Epoch [3001/10000], Loss: 2.067976e-04
Epoch [4001/10000], Loss: 5.161781e-04
Epoch [5001/10000], Loss: 1.347004e-04
Epoch [6001/10000], Loss: 1.945561e-04
Epoch [7001/10000], Loss: 2.020037e-04
Epoch [8001/10000], Loss: 6.068760e-05
Epoch [9001/10000], Loss: 2.144208e-05
```
{: file='MLP-pytorch - result'}

<p align="center">
<iframe src="https://drive.google.com/file/d/164AX2dFmliSviKPqp7PiISQXLZSY2RGB/preview" width="350" height="281" allow="autoplay"></iframe>
  <br>
  <em>Figure 3. Result: Multi-layer perceptron </em>
</p>

## Conclusion

In this article, we explored three techniques for data interpolation: Lagrange interpolation, natural cubic spline, and multi-layer perceptron (MLP). Lagrange interpolation, the simplest approach, can sometimes produce spurious spikes or erratic fluctuations near the edges of the dataset due to rapid oscillations in the polynomial function. This phenomenon, often referred to as the Runge phenomenon, can be mitigated by carefully selecting data points, such as Chebyshev's nodes. Natural cubic spline interpolation ensures smoothness by enforcing continuity in the first and second derivatives across the domain. While the multi-layer perceptron demonstrates effective data fitting, its interpretability poses challenges, making it a somewhat tricky method to employ.

## Reference
[Wikipedia-Lagrange polynomial](https://en.wikipedia.org/wiki/Lagrange_polynomial)<br>
[Wikipedia-Spline](https://en.wikipedia.org/wiki/Spline_(mathematics)#:~:text=A%20common%20spline%20is%20the,of%20the%20interval%20of%20interpolation.)<br>
[Wikipedia-MLP](https://en.wikipedia.org/wiki/Multilayer_perceptron)<br>
[Wikipedia-Chebyshev nodes](https://en.wikipedia.org/wiki/Chebyshev_nodes)<br>


