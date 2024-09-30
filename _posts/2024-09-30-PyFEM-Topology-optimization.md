---
title: PyFEM - topology optimization
description: An explanation of PyFEM topology optimization code and displaying a result.
author: chulmin
date: 2024-09-29 00:00:00 -0500
categories: [FEM, Topology optimization]
tags: [FEM, Finite element method, Topology Optimization]
math: true
---

## PyFEM
I developed PyFEM, a finite element method (FEM) code specifically designed for topology optimization this time. The main goal behind creating this code is to provide a straightforward and practical tool for readers and learners who want to deepen their understanding of the finite element method. Implemented in Python using basic libraries and NumPy for numerical computations, PyFEM aims to simplify the complexities of FEM simulations, making it especially accessible for those interested in topology optimization.

For readers unfamiliar with the code, you can refer to [the previous article](https://chulminator.github.io/posts/PyFEM-elastoplasticity/) for an introduction.

## Topology optimization
Topology optimization seeks to identify the most efficient distribution of material within a structure, ensuring it meets its functional requirements while minimizing material usage. This approach is essential for enhancing efficiency and reducing costs.

For instance, consider the scenario of 3D-printing an arm cast in a spaceship (Figure 1). In this context, minimizing weight is critical, as every kilogram launched into space significantly increases costs. Therefore, using a traditional HEAVY gypsum cast would be impractical and expensive. The optimized design not only protects the injured area effectively but also contributes to overall weight reduction, making it a more viable solution for space missions.

<p align="center"> <iframe src="https://drive.google.com/file/d/1TN3Dr__GtgheafTh3Ida96VmeU6h4DDb/preview" width="435" height="368" allow="autoplay"></iframe> <br> <em>Figure 1. 3D printing a cast in the SF movie "Stowaway"</em> </p>

In this article, we adopt the approach of using a pseudo-density function that takes values between 0 and 1, representing the material density at different points. The topology optimization problem involves finding an optimal distribution of this pseudo-density by considering the objective function, such as minimizing compliance or weight, while fulfilling design constraints like volume fraction limits.

To find the optimal material distribution, we penalize intermediate density values (those between 0 and 1) to encourage a clear 0/1 material distribution, thereby avoiding intermediate states that have low structural efficiency. The optimization process relies on calculating the gradient of the objective function with respect to the material volume, which involves iteratively updating the pseudo-density to find an optimal solution that meets the requirements of the problem .


## Messerschmitt-Bölkow-Blohm (MBB) beam problem

<p align="center"> <iframe src="https://drive.google.com/file/d/1SyInfjNlXiXOGKEMJDC7le-AOoCCq1Xu/preview" width="696" height="333" allow="autoplay"></iframe> <br> <em>Figure 2. Schematics of MBB beam problem"</em> </p>

### input
```text
*Title
MBB_beam_15201_30000

*Mesh
./Mesh/MBB_beam/MBB_beam_15201_30000

*ConstitutiveLaw
Solid
linearelasticity
1.0	0.3

*BCFile
BDC.BC_MBB_beam

*Plane
PlaneStress	1.0

*PlasticModel
PlasticModel.LinearElastic

*ResultDirectory
./Result/result_MBB_beam/15201_30000
```

### Result
{% include embed/youtube.html id='v1qj0oTh_qA' %}


## Reference
- [https://github.com/Chulminator/PyFEM_TopologyOptimization](https://github.com/Chulminator/PyFEM_TopologyOptimization)


This code is implementd based on this paper.
- [https://paulino.princeton.edu/software.html](https://paulino.princeton.edu/software.html)
- [Talischi, C., Paulino, G.H., Pereira, A. and Menezes, I.F., 2012. PolyTop: a Matlab implementation of a general topology optimization framework using unstructured polygonal finite element meshes](https://paulino.princeton.edu/journal_papers/2012/SMO_12_PolyTop.pdf)
