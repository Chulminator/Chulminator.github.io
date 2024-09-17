---
title: Recasting material constitutive model using Graph Neural Network
author: chulmin
date: 2024-09-16 00:00:00 -0500
categories: [GNN]
tags: [GNN, PyG, constitutive model]
math: true
---

## Motivation
Graph neural networks (GNNs) can be considered an advanced form of convolutional neural networks (CNNs). While CNNs operate by utilizing the values of neighboring pixels, GNNs allow for the manual selection of which neurons are considered neighbors. This distinction makes CNNs particularly effective for handling regular datasets, such as images, whereas GNNs excel in managing irregular datasets as well as regular datasets across spatial and temporal dimensions. In this article, we leverage the unique characteristics of graph neural networks to reformulate the material constitutive model.

## Hyperelastic model 
<p align = "center">
<iframe src="https://drive.google.com/file/d/1No8jbFpUPcp0mU2sJndAPQxhNrKXNQ36/preview" width="500" height="240" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Hyperelastic material model </em>
</p> 

### Training step
1. Randomly generate strain values, ($\varepsilon_{ij}$), within the desired range
2. Calculate the volumetric strain, ($\varepsilon_{v}$), and deviatoric strain, ($\varepsilon_{s}$), at each strain point.
3. Randomly select a number of data points, ranging from 3 to 10
4. Construct a graph using Delaunay triangulation in the ($\varepsilon_{v}$, $\varepsilon_{s}$) space

<p align = "center">
<iframe src="https://drive.google.com/file/d/1Npl5DzPCnVd8bfG8BpF9KvjzDQTA6fks/preview" width="510" height="477" allow="autoplay"></iframe>
  <br>
  <em>Figure 2. Structure of GNN </em>
</p> 



<p align = "center">
<iframe src="https://drive.google.com/file/d/1OWw0-nObIeYXFBFexR08Y3k8OOGW9BnF/preview" width="829" height="217" allow="autoplay"></iframe>
  <br>
  <em>Figure 3. Loss curve for training 1, 2, and 3 </em>
</p> 

Three cases were analyzed as indicated in Figure 3:
1. **First Analysis**: 
   - Utilizes the \( $L_2$ \) loss with the strain energy at the state.

2. **Second Analysis**: 
   - Also employs the \( $L_2$ \) loss, but with the stress state at each strain state.

3. **Third Analysis**: 
   - Implements Sobolev training, using the \( $L_2$ \) loss with the strain energy and its derivative, which should be equal to the stress.
   
The information is summarized in the following table:


| case       | Loss |
| :--------- | ----:---- |
| Training 1 | $  \mathcal{L} = \text{MSE}( W^{\text{pred} } - W^{\text{ref} } ) $         |
| Training 2 | $  \mathcal{L} = \text{MSE}( \boldsymbol{S}^{\text{pred} } - \boldsymbol{S}^\text{ref} ) $         |
| Training 3 | $  \mathcal{L} = \text{MSE}( W^{\text{pred} } - W^{\text{ref} } ) + \text{MSE}( \dfrac{\partial W^{\text{pred} } }{\partial \boldsymbol{E}} - \boldsymbol{S}^\text{ref} ) $         |

### Result
<p align = "center">
<iframe src="https://drive.google.com/file/d/1OTGisHRNwiI3FUB9y9MrEmuzYmu__q1n/preview" width="829" height="217" allow="autoplay"></iframe>
  <br>
  <em>Figure 4. Result </em>
</p> 

In Figure 4, all three analyses align closely with the analytical solution. However, in the second and third figures, the first analysis exhibits the largest absolute error for both strain energy and stress. The second analysis is unable to compute the strain energy under the given conditions, which prevents the calculation of its absolute error for strain energy, but it does show a smaller absolute error for stress. In contrast, the third analysis, utilizing Sobolev training, demonstrates the smallest absolute errors for both strain energy and stress.

## ElastoPlastic model
J2 Plasticity (see [**PyFEM**](https://chulminator.github.io/posts/PyFEM/))
### Training step
1. Randomly generate a loading path
2. Generate the dataset by Randomly selecting data points from the loading path

<p align = "center">
  <iframe src="https://drive.google.com/file/d/1OYn96xfeHk2lTRdmwYzyzbnyBAwsAzCA/preview" width="433" height="359" allow="autoplay"></iframe>
  <!-- <iframe src="https://drive.google.com/file/d/1OCmeq0UkIMn0f4Ug30GXXxica9DEOrC_/preview" width="432" height="400" allow="autoplay"></iframe> -->
  <iframe src="https://drive.google.com/file/d/1OBIz9jALLNIxygrhhl3JEInhIU2R0fwD/preview" width="415" height="400" allow="autoplay"></iframe>
  <br>
  <em>Figure 4. Example of a loading path and input and output for GNN </em>
</p> 

### Result
<p align = "center">
  <iframe src="https://drive.google.com/file/d/1OEetdHpcSCsrWmhKCUESJYIeILPNgEUT/preview" width="828" height="213" allow="autoplay"></iframe>
  <br>
  <em>Figure 5. Loss curve, strain stress curve, and plastic multiplier loading step curve </em>
</p> 

As shown in Figure 5, the graph neural network could effectively capture the behavior of history-dependent material, demonstrating its ability to model complex relationships and dependencies over time. 

## Conclusion
- The features of Graph Neural Networks (GNNs) enable effective recasting of constitutive models
    - Self-loops and directed graphs are employed to represent history-dependent material models
- GNNs are capable of handling irregular data
- Sobolev training enhances the quality of training by imposing physical laws into the neural networks

## Reference
https://pytorch-geometric.readthedocs.io/en/latest/


