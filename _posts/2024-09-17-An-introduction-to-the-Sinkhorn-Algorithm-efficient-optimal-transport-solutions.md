---
title: An introduction to the Sinkhorn Algorithm - efficient optimal transport solutions
author: chulmin
date: 2024-09-17 00:00:00 -0500
categories: [Algorithm]
tags: [Sinkhorn, algorithm, optimal transport]
math: true
---

## Optimal transport
Optimal transport begins with a fundamental question: How can we efficiently transfer items from one location to another? To illustrate this, let’s consider an example problem:

### Example
During the pandemic, the demand for instant noodles skyrocketed. In towns A, B, and C, each store has 1,000 packs of instant noodles. The distance between each of these towns is equal to one unit. Additionally, the demand for instant noodles is evenly distributed across the towns. As a store owner, the question is: What is the most efficient way to distribute the noodles while minimizing.

<p align = "center">
<iframe src="https://drive.google.com/file/d/1Ooq_kV5ktgTwR7DyhY4IUrQk4HYEp9h_/preview" width="700" height="258" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Example problem for the optimal transport </em>
</p> 

The question now becomes: What is the most efficient way to distribute the noodles while minimizing transportation costs? To approach this, we can construct a cost matrix that represents the transportation cost of moving instant noodles from one town to another. The cost matrix is defined as follows:

$$
\begin{equation}
  C_{ij} = \begin{bmatrix}
	    0 &1 & 1 \\
	    1 &0 & 1 \\
	    1 &1 & 0 
    \end{bmatrix}
  \label{eq:costMatrix}
\end{equation}
$$

where $C_{ij}$ represents the cost of transporting from town i to town j, with 1, 2, and 3 corresponding to towns A, B, and C, respectively. Interpreting the cost matrix, it costs zero to transfer noodles within the same town (e.g., from town A to town A), but the cost is one to transfer noodles between different towns (e.g., from town A to town B or town C), and similarly for the other towns.

So, what would be the most efficient way to manage the transportation? Clearly, the optimal strategy is to sell the noodles directly in the town where they are stored. In other words, each town should sell its own supply locally to minimize transportation costs. This strategy can be referred to as the transportation plan.

This transportation plan can also be represented mathematically using a transportation matrix (also called permutation matrix in other context):

$$
\begin{equation}
  P_{ij} = \begin{bmatrix}
	   1000 &0 & 0 \\
	    0 & 1000 & 0 \\
	    0 & 0 & 1000 
    \end{bmatrix}
  \label{eq:permMatrix1}
\end{equation}
$$

However, for various reasons, one might set the transportation matrix in a different way. For example, consider the following transportation matrix:

$$
\begin{equation}
  P_{ij} = \begin{bmatrix}
	   576 &212 & 212 \\
	    212 & 576 & 212 \\
	    212 & 212 & 576 
    \end{bmatrix}
  \label{eq:permMatrix2}
\end{equation}
$$

In this case, each town transfers part of its supply to the other towns.

Finally, we can calculate the total transportation cost for both the first and second transportation plans by performing element-wise multiplication of the cost matrix and the transportation matrix. For the first transportation plan, the total transportation cost is zero, as expected, since each town sells its own supply locally. This is the most optimal solution. For the second transportation plan, where goods are partially transferred between towns, the total transportation cost is 1,272.

## Sinkhorn algorithm
The optimal transport problem can be mathematically represented as follows:

$
\begin{equation}
  \text{Find a transportation matrix} \; P_{ij} \; \text{that minimizes loss} \;\mathcal{L}  =  \sum\limits_{i,j} C_{ij} P_{ij}
\end{equation}
$

To regularize the problem, we introduce the supply and demand, denoted by $r_i$ and $c_j$, respectively. These are normalized to 1, leading to the following constraint:

$
\begin{equation}
  \sum\limits_{i} r_i = 1 \; \text{and} \; \sum\limits_{j} c_j = 1
  \label{eq:constraint1}
\end{equation}
$

Additionally,

$
\begin{equation}
  \sum\limits_{j} P_{ij} = r_i \; \text{and} \; \sum\limits_{i} P_{ij} = c_j
  \label{eq:constraint2}
\end{equation}
$

These constraints are intuitive, as they ensure that the total supply and demand in the transport problem are satisfied, as demonstrated in the earlier example.

However, solving the optimal transport problem can be challenging because it is not always convex, which makes finding the optimal solution difficult. The Sinkhorn algorithm addresses this by adding an extra regularization term to ensure convexity:
$
\begin{equation}
    \mathcal{L} = \sum\limits_{i,j} C_{ij} P_{ij} + \lambda P_{ij} \log{P_{ij}}
  \label{eq:Sinkhorn}
\end{equation}
$

Here, $\lambda$ is a regularization parameter that controls the degree of convexity. The additional term, $\lambda P_{ij} \log{P_{ij}}$,is indeed convex considering elements of $P_{ij}$ are between 0 and 1.

The specific derivation of the Sinkhorn algorithm involves taking the derivative of the loss function with respect to $P_{ij}$, which leads to an iterative method. The detailed derivation can be found in the relevant literature.

## Example with Sinkhorn algorithm
Let’s consider an example where there are three cement factories: A, B, and C. These factories produce a certain amount of cement, which needs to be transported to four construction sites: a, b, c, and d. The question is: What is the optimal way to transport the cement from the factories to the construction sites, minimizing the overall transportation cost?


<p align = "center">
<iframe src="https://drive.google.com/file/d/1OcljYfNfmsWYs658EyCuvsHqB0wyQwWC/preview" width="700" height="623" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. Example problem for the Sinkhorn algorithm </em>
</p> 

The cost matrix, according to Figure 2, is calculated as:

$$
\begin{equation}
    C_{ij} = \begin{bmatrix}
        3.1623 & 7.0711 & 7.0711 & 6.3246 \\
        4 & 7.2111 & 6.3246 & 5.831 \\
        5.3852 & 7.2801 & 5 & 5
    \end{bmatrix}
    \label{eq:cost}
\end{equation}
$$

The code is provided here:

```python
# Import necessary packages and functions
import numpy as np
import matplotlib.pyplot as plt

# Code is slightly changed from
# https://www.pragmatic.ml/sparse-sinkhorn-attention/


def sinkhorn_knopp(cost_matrix, source, target, reg):
    # Largest entries of P correspond to
    # movements with lowest cost
    eps=1e-4

    P = np.exp(-cost_matrix / reg)
    P /= P.sum()

    # Source corresponds to rows,
    # target corresponds to colums
    source = source.reshape(-1, 1)
    target = target.reshape(1, -1)

    err = 1
    ii = 0
    P_prev = np.copy(P)
    while err > eps:
        ii +=1
        # Over time this both the row_ratio and
        # col_ratio should approach vectors of all 1s
        # as our transport matrix approximation improves
        row_ratio = source / P.sum(axis=1, keepdims=True)
        P *= row_ratio
        col_ratio = target / P.sum(axis=0, keepdims=True)
        P *= col_ratio

        err = np.linalg.norm(P_prev-P,"fro")
        P_prev = np.copy(P)

    min_cost = np.sum(P * cost_matrix)
    return P, min_cost

source_dist = np.array([0.2, 0.3, 0.5])
target_dist = np.array([0.2, 0.35, 0.3, 0.15])
	    

cost_matrix = np.array([[3.1623, 7.0711, 7.0711, 6.3246 ],
                        [4, 7.2111, 6.3246, 5.831],
                        [5.3852, 7.2801, 5, 5 ]])

print("\ncost_matrix\n",cost_matrix)

transport_matrix, min_cost = sinkhorn_knopp(
    cost_matrix,
    source_dist,
    target_dist,
    reg=0.1
)

transport_matrix *= 2000
print("\nTransport matrix\n", transport_matrix)
print("\nMin cost\n", np.sum(transport_matrix * cost_matrix))

transport_matrix, min_cost = sinkhorn_knopp(
    cost_matrix,
    source_dist,
    target_dist,
    reg=0.5
)

transport_matrix *= 2000
print("\nTransport matrix\n", transport_matrix)
print("\nMin cost\n", np.sum(transport_matrix * cost_matrix))


transport_matrix, min_cost = sinkhorn_knopp(
    cost_matrix,
    source_dist,
    target_dist,
    reg=1.0
)

transport_matrix *= 2000
print("\nTransport matrix\n", transport_matrix)
print("\nMin cost\n", np.sum(transport_matrix * cost_matrix))
transport_matrix, min_cost = sinkhorn_knopp(
    cost_matrix,
    source_dist,
    target_dist,
    reg=0.0001)

transport_matrix *= 2000
print("\nTransport matrix\n", transport_matrix)
print("\nMin cost\n", np.sum(transport_matrix * cost_matrix))
```


<p align = "center">
<iframe src="https://drive.google.com/file/d/1Opv_I2vM9XoRSCNj_dxrM3TvAY7j-izW/preview" width="841" height="289" allow="autoplay"></iframe>
  <br>
  <em>Figure 3. Result </em>
</p> 


### Conclusion
- A smaller value of $\lambda$ results in a lower transportation cost.
    - As shown in Figure 3, the smallest $\lambda$ leads to the lowest transportation cost, while larger $\lambda$ values produce a more dispersed transportation plan.
- However, setting $\lambda$ too small can result in NaN values.
    - Intuitively, this happens because dividing by a very small regularization term can cause the values to become excessively large.
- The Sinkhorn algorithm is commonly used in neural network training, especially in applications involving optimal transport.

## Remark 
This code can be integrated with the PyTorch framework for use in machine learning applications.

```python
import torch
import torch.nn as nn
import matplotlib.pyplot as plt

class SinkhornDistance(nn.Module):
    def __init__(self, eps, reg, max_iter):
        super(SinkhornDistance, self).__init__()
        self.eps      = eps
        self.max_iter = max_iter
        self.reg      = reg

    def forward(self, cost_matrix, source, target):
        # size of source: N
        # size of target: K
        # size of CostMatrix: K by N

        M = torch.exp(-cost_matrix / self.reg)
        u = torch.ones_like(source) # size of u: N
        v = torch.ones_like(target) # size of v: K

        err = 1
        ii = 0
        P = torch.diag(u) @ M @ torch.diag(v)
        P_prev = torch.clone(P)
        for ii in range(self.max_iter):

            ii +=1
            u = source/(M@v)
            v = target/(M.T@u)

            P = torch.diag(u) @ M @ torch.diag(v)

            err = torch.linalg.norm(P_prev-P,"fro")
            if err < self.eps:
                break
            P_prev = torch.clone(P)
        min_cost = torch.sum(P * cost_matrix)
        return P, min_cost
```

## Reference
https://regularize.wordpress.com/2015/09/17/calculating-transport-plans-with-sinkhorn-knopp/
https://dfdazac.github.io/sinkhorn.html
https://www.pragmatic.ml/sparse-sinkhorn-attention/n
https://amsword.medium.com/a-simple-introduction-on-sinkhorn-distances-d01a4ef4f085
Cuturi, M., 2013. Sinkhorn distances: Lightspeed computation of optimal transport. Advances in neural information processing systems


