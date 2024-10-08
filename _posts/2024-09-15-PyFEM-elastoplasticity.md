---
title: PyFEM - elastoplasticity
author: chulmin
date: 2024-09-15 00:00:00 -0500
categories: [FEM, Elasto-plasticity]
tags: [FEM, Finite element method, ElastoPlasticity]
math: true
---

## PyFEM
I developed PyFEM, a finite element method (FEM) code designed to simulate the behavior of elasto-plastic materials. The primary motivation behind creating this code is to provide a clear and practical tool for readers and learners who seek a better understanding of the finite element method. This code is implemented in Python using basic libraries and NumPy for numerical computations. PyFEM aims to demystify the process of FEM simulations, especially for those interested in material behavior under elastic and plastic conditions.

In this article, I will explain how to run the code with various configurations and guide you through the process of verification. The verification process is an essential step in ensuring the correctness, and I will also cover how to prepare input files, which are used to define simulation scenarios such as material properties, boundary conditions, and loading steps.

## Input files
- Input files must have the extension .inp to be recognized by PyFEM as valid simulation files.
- These files contain the essential details that define the finite element model, including the geometry, material behavior, and the simulation setup.
- Each section in the input file begins with an asterisk (*), which helps in organizing the different components of the simulation such as material definitions and boundary conditions.
- Example
    - **\*Title**: The title of the analysis.
    - **\*ConstitutiveLaw**: Defines the material properties.
        - The first line currently supports only the `solid` command.
        - The second line currently supports only the `elastoPlasticity` command.
        - The third line specifies material properties; the first and second numbers represent $E$ (Young's modulus) and $\mu$ (Poisson's ratio).
    - **\*Plane**: This assumption is required only for 2-dimensional analysis and can be either `PlaneStrain` or `PlaneStress`. Additionally, the second number indicates the width.
    - **\*ResultDirectory**: The directory where the result will be saved.
    - **\*LoadingStep**: Specifies the number of steps used in the analysis.
    - **\*BCFile**: User-defined boundary conditions for the analysis. A detailed description will be provided in the next section.
    - **\*ReturnMappingMode**: `EigenSpace`, `TensorSpace`, and `PQspace` are available.


```text
*Title
Patch_UniaxialTension_J2_142_242_PE

*Mesh
./Mesh/Patch/Patch_142_242

*ConstitutiveLaw
Solid
ElastoPlasticity
200000.0	0.3	250.0	0.2

*LoadingStep
100

*BCFile
BDC.BC_Patch_UniaxialTension

*Plane
PlaneStrain	1.0

*ResultDirectory
./Result/result_Patch_UniaxialTension_PE/142_242_eigenspace

*PlasticModel
PlasticModel.J2Plasticity

*ReturnMappingMode
EigenSpace


```

## Boundary Condition

- The Python function name in the boundary condition file should remain unchanged from "ApplyBC."
- Example
    - **BC_E** indicates the essential boundary condition.  
    - The number in `x[0]` (zero in this case) indicates the direction of the coordinates. `0` and `1` correspond to the x and y directions, respectively.  
    - `Dim = 2` for 2-dimensional analysis and `Dim = 3` for 3-dimensional analysis. 
<p align = "center">
<iframe src="https://drive.google.com/file/d/1MuAOIo-OTB6cbMapmVtzGcBSOM9xPD9m/preview" width="320" height="344" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Boundary condition for the Uniaxial tension test</em>
</p> 

``` python
def ApplyBC(Fem, Node, Element): # Boundary condition for the Uniaxial tension test
    Dim = Fem.Dimension
    for ind, x in enumerate(Node.Coord):
        Ndof = NodeDof(Node, [Node.Id[ind]])
        if math.fabs(x[1] - 0.0 ) < 1e-05: # red color part in Figure 1
            Node.BC_E[Ndof[0]*Dim+1] = 0.0
        if math.fabs(x[0] - 0.0 ) < 1e-05: # blue color part in Figure 1
            Node.BC_E[Ndof[0]*Dim] = 0.0
        if math.fabs(x[1] - 1.0 ) < 1e-05: # green color part in Figure 1
            Node.BC_E[Ndof[0]*Dim+1] = 0.03/Fem.totalstep
    return
```

## Elasto-Plastic Material Model

- Similar to the boundary condition, the function in `PlasticModel` should not be changed from "MyPlasticity."  
- Material properties are stored in the `Fem.MatProp` list. The length of the list corresponds to the number of material properties.  
    - In the example input file, `Fem.MatProp[0]`, `Fem.MatProp[1]`, `Fem.MatProp[2]`, and `Fem.MatProp[3]` will be `200000`, `0.3`, `250`, and `0.2`, respectively.  
- Additionally, the plasticity model must include the yield surface and its first and second derivatives in the given coordinate system for the return mapping algorithm. 
- Example - J2 isotropic hardening law 

<p align = "center">
<iframe src="https://drive.google.com/file/d/1MnEpM-g3xMmANZZfcP9FbM4gb6h-MRbp/preview" width="320" height="297" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. J2 plasticity yield surface and its flow direction </em>
</p>

``` python
class MyPlasticity():
    def __init__(self, Fem):
        self.K  = Fem.K
        self.mu = Fem.mu
        self.E  = Fem.E
        self.nu = Fem.nu
        self.twoD = Fem.twoD
        self.sigma_y = Fem.MatProp[2]
        self.n_hard  = Fem.MatProp[3]

    def f(self, sigma1, sigma2, sigma3, lamda):
        N = 2.0
        n_hard  = self.n_hard
        sigma_y = self.sigma_y
        E = self.E
        phi = (1.0/2.0)*(np.abs(sigma1-sigma2)**N + \
                         np.abs(sigma2-sigma3)**N + \
                         np.abs(sigma3-sigma1)**N)
        kappa = sigma_y*(1 + E*lamda/sigma_y)**n_hard
        return phi**(1/N) - kappa
        
    def df(self, sigma1, sigma2, sigma3, lamda):
        N = 2.0
        n_hard  = self.n_hard
        sigma_y = self.sigma_y
        E = self.E
        x, y, z= sigma1, sigma2, sigma3
        denom = np.sqrt(2)*((x-y)**2+(y-z)**2+(z-x)**2)**(1/2)
        dfdsig1  = (2*x-y-z)/ denom
        dfdsig2  = (2*y-z-x)/ denom
        dfdsig3  = (2*z-x-y)/ denom
        dfdlamda = -n_hard*E*(1.0 + E*(lamda)/sigma_y)**(n_hard-1) 
        return dfdsig1, dfdsig2, dfdsig3, dfdlamda

    def df2(self, sigma1, sigma2, sigma3):
        x, y, z= sigma1, sigma2, sigma3
        denom = np.sqrt(2)*((x-y)**2+(y-z)**2+(z-x)**2)**(3/2)
        if denom < 1e-6:
            denom = 1e-6
        d2fdsig1dsig1 = 3*(y - z)**2 / denom
        d2fdsig2dsig2 = 3*(z - x)**2 / denom
        d2fdsig3dsig3 = 3*(x - y)**2 / denom
        d2fdsig1dsig2 = 3*(z-x)*(y-z) / denom
        d2fdsig2dsig3 = 3*(x-y)*(z-x) / denom
        d2fdsig3dsig1 = 3*(y-x)*(z-y) / denom
        return d2fdsig1dsig1, d2fdsig2dsig2, d2fdsig3dsig3,\
                d2fdsig1dsig2, d2fdsig2dsig3, d2fdsig3dsig1
```

## Verification
- Uniaxial tension test for linear elasticity
    - Material properties
        - $E = 200000 [\text{MPa}]$ 
        - $\mu = 0.3 $
    - Figure 3 shows the boundary condition of the biaxial tension test.
    - Result: the example is verified in 4 cases.
        - Figure 4 illustrates that the stress increases linearly as the strain increases.

|             | # of nodes | # of elements |
| :---------- | --------: | ------: |
| 2D - Q4     | 4         | 1       |
| 2D - T3     | 142       |  242    |
| 3D - Hex8   | 8         |  1      |
| 3D - Hex8   | 216       |  125    |
    
<p align = "center">
<iframe src="https://drive.google.com/file/d/1Mmos8foSJXNKSNkNy6zG1_93AtsZznjq/preview" width="307" height="320" allow="autoplay"></iframe>
<iframe src="https://drive.google.com/file/d/1MYfN1cw6BglJJoz8EnHu0ixQDoaCNu5U/preview" width="346" height="280" allow="autoplay"></iframe>
  <br>
  <em>Figure 3. Schematics of uniaxial tension test in 2D and 3D </em>
</p>

<p align="center">
    <iframe src="https://drive.google.com/file/d/1N2g0OMDPEB9JkoC74xtUeWKHnYasKipE/preview" width="320" height="257" allow="autoplay"></iframe>
  <br>
  <em>Figure 4. Result - Uniaxial tension test for linear elastic material model</em>
</p>

- Uniaxial tension test for J2 isotropic hardening 
    - Figure 5 illustrates that the stress increases linearly as the strain increases before the Von Mises stress reaches the yield stress.

<p align="center">
    <iframe src="https://drive.google.com/file/d/1N0jGcBeFU2MPE7OJsm6fUCi8UcEK58Ah/preview" width="320" height="257" allow="autoplay"></iframe>
  <br>
  <em>Figure 5. Result - Uniaxial tension test for J2 isotropic hardening plastic material model</em>
</p>

- Biaxial tension test for J2 isotropic hardening 
    - Figure 6 shows the boundary condition of the biaxial tension test.
    - Figure 7 illustrates that the stress increases linearly as the strain increases before the Von Mises stress reaches the yield stress.

<p align="center">
    <iframe src="https://drive.google.com/file/d/1MWfIbCumdXPeyifceORYSo3CTVtlQTBC/preview" width="307" height="313" allow="autoplay"></iframe>
    <iframe src="https://drive.google.com/file/d/1MQ3F0kgDW2XPTo1u-ScS71beoJdRfsj5/preview" width="342" height="279" allow="autoplay"></iframe>
  <br>
  <em>Figure 6. Schematics of biaxial tension test in 2D and 3D </em>
</p>

<p align="center">
    <iframe src="https://drive.google.com/file/d/1N2uJ3L51dV_31XQrvwA4zIPZC-oHjASQ/preview" width="320" height="257" allow="autoplay"></iframe>
  <br>
  <em>Figure 7. Result - Biaxial tension test for J2 isotropic hardening plastic material model</em>
</p>

## Remark
- All the input files are included in [**Github repository**](https://github.com/Chulminator/PyFEM_ElastoPlasticity)
- Dependency:
    - numpy
    - scipy
    - pandas
    - mesh.io

## Reference
Borja, R.I., 2013. Plasticity. Springer Berlin Heidelberg. doi: 10.1007/978-3-642-37310-0.

