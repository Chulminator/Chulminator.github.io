---
title: How to code tetris board and block?
author: chulmin
date: 2024-03-20 00:00:00 -0500
categories: [SFML, Tetris]
tags: [SFML, game, 2D, Tetris, block, board]
---


## Tetris board and active block
In the original Tetris game, the board is traditionally set at a size of 20 by 10 squares. However, based on the discussions from the [previous post](https://chulminator.github.io/posts/A-Developer's-Journey-into-Game-Creation!-(Tetris)/), we've opted for different dimensions, specifically 19 by 11. The Tetris board is represented as a two-dimensional integer array, with additional lines inserted at the left, right, and bottom to serve as walls and a floor. There are seven different types of blocks for the active piece that the player can control, denoted as T, I, S, Z, L, J, and SQ in my code. To visualize these block types, I've utilized a local 2D array with a size of 4 by 4.

<br>
<center>
	<iframe src="https://drive.google.com/file/d/10eoe-kg3T-2kmIDWxtlMwiALLPZ20Rik/preview" width="640" height="325" allow="autoplay"></iframe>
	<br>
  <em>Figure 1. 7 different types of blocks in the local board; (a) T shape, (b) I shape, (c) S shape, (d) Z shape, (e) L shape, (f) J shape, and (g) SQuare shape.</em>
</center> 
<br>

The movement of the block to the left, right, and downward takes place by relocating the location of the local array, whose origin of the local and global board is the left and top. Collision detection between the active block and the global board is determined by calculating the global coordinate of the active block on the board, using both the global position of the local board and the local position of the block within the local board. Additionally, clockwise rotation of the block is performed within the local board:

```cpp
void TetrisBlock::rotateBlock(unsigned char rotatedBlock[][2]){	
	unsigned char MinValueY = 4;
	unsigned char MinValueX = 4;
	
	for (int ii = 0; ii < 4; ii++){
		rotatedBlock[ii][0] = 4 - block[ii][1] - 1;
		rotatedBlock[ii][1] = block[ii][0];
		if (rotatedBlock[ii][1] < MinValueY){
			MinValueY = rotatedBlock[ii][1];
		}
		if (rotatedBlock[ii][0] < MinValueX){
			MinValueX = rotatedBlock[ii][0];
		}
	}
	for (int ii = 0; ii < 4; ii++){
		rotatedBlock[ii][0] -= MinValueX;
	}
}
```
{: file='TetrisBlock.cpp'}

MinValueX and MinValueY are used to prevent the block from moving beyond the boundaries to the right or downward after rotation. In the code, the variable "block" represents the local 2D array. However, the block is not directly rotated to ensure that the rotated block is compatible with the board, including elements such as the floor, walls, and inactive blocks.
<br>
<p align = "center">
	<iframe src="https://drive.google.com/file/d/10d7bi6CzCefwZFzV78JhqvZZktJ76QJg/preview" width="640" height="457" allow="autoplay"></iframe>
	<br>
  <em>Figure 2. Schematics of the interaction between the local board and the global board </em>
</p>
<br>

## Collision and dropping
Movement and rotation occur at the block level, whereas collision detection takes place at the board level due to the interaction between the board and the block. Checking for collisions during left and right movements is straightforward. This involves verifying whether the coordinates of the four components of the block intersect with a wall or an inactive block after adjusting the local board's x-direction coordinates by +1 or -1.

<p align = "center">
	<iframe src="https://drive.google.com/file/d/10hWwJ9sAAOy9w7otriTOC3GFQ1zJ2P5S/preview" width="700" height="170" allow="autoplay"></iframe>
	<br>
  <em>Figure 3. Collision detection after rotation </em>
</p>

Verifying collisions after rotation necessitates an additional step, as illustrated in Figure 3. For instance, if we have an I-shaped block positioned at the far right of the board (Figure 3(a)), upon rotation, a collision is detected, represented by the red component in Figure 3(b). However, despite the collision, ample space exists on the left side of the local block. Thus, it becomes essential to verify if the block is admissible when translated to the left or upwards (Figure 3(c)).

Finally, we address the verification of dropping. Like collision detection, dropping is a straightforward process during movement. Since they share similar functions, they can be handled together. Dropping involves moving the local board in the y direction, unlike collision detection which operates in the x direction. However, confirming whether the active block has been dropped involves deactivating it and spawning a new one. Therefore, verifying dropping is treated separately from collision detection.

## Conclusion
Not all technical aspects of the code are elaborated on in this post. Instead, the focus is on considerations for implementing the Tetris mechanism.

## Reference 
[Haller, J. and Hansson, H.V., 2013. SFML Game Development. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development)<br>
[Pupius, R., 2015. SFML Game Development By Example. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development-by-example)



