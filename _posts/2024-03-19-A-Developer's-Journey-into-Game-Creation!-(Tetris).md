---
title: A Developer's Journey into Game Creation! (Tetris)
author: chulmin
date: 2024-03-19 00:00:00 -0500
categories: [Game, SFML, Tetris]
tags: [SFML, game, 2D, Tetris]
---

For my upcoming series of posts, I will be documenting the process of developing the "Tetris" game from start to finish. Although these posts were not written in real-time during the development process, I have made an effort to capture my thoughts and considerations at each stage. Join me on this journey as I share the insights and challenges encountered while bringing this classic game to life.


## Copyright of Tetris
My initial concern was whether it is legally permissible to mimic the original game Tetris and share it publicly. While this is merely an exercise, addressing this question is crucial as similar issues may arise with various elements such as photos, sounds, and games themselves.


Indeed, there have been numerous debates on the issue, with one fundamental question being raised: "What truly defines Tetris?" In an article discussing the matter (source: Arstechnica), the author asserts that the abstract ideas and rules that form the foundation of Tetris cannot be protected by copyright:

- The player controls the block until it touches the floor. 
- These blocks accumulate at the top of the screen.

However, certain details of the original game should be protected, including the dimensions of the board, the presence of a shadow piece on the floor, the display of the next block, and the appearance of squares automatically filling the game board when the game is over.


## What to include and consider
- Random generation of the blocks
- Collision (not to penetrate the static blocks, floor, and wall)
- Removing lines once the line is filled
- Decoration of the page
- Sound effect
- Release version


## Role of each source code
- Game.cpp: This source code file handles the technical aspects of the game, including input events such as keyboard or mouse inputs. It is responsible for capturing and processing user interactions to control the game.
- World.cpp: This source code file manages the different pages or screens of the game. It handles the transitions between different game states, such as the main menu, level selection, or game over screen. It ensures a smooth flow and provides the overall structure of the game.
- Tetris.cpp: This source code file contains functions specifically related to the Tetris board. It handles the logic and mechanics of the game, including the placement and interection with the active block, checking for completed lines, and updating the game state accordingly.
- TetrisBlock.cpp: This source code file focuses on functions related to the active Tetris block. It manages the creation, rotation, and movement of the Tetris blocks within the game. It ensures that the blocks interact properly with the Tetris board and other game elements.

## Conclusion
These are the set of attributes in mind initially. I couldn't strictly adhere to it. Some attributes were created, while others were edited or even removed along the way. The development process naturally involves adjustments and modifications based on new insights and project requirements.


## Reference 
[Haller, J. and Hansson, H.V., 2013. SFML Game Development. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development)<br>
[Pupius, R., 2015. SFML Game Development By Example. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development-by-example)



