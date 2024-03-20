---
title: Randomness in game
author: chulmin
date: 2024-03-21 00:00:00 -0500
categories: [SFML, Tetris]
tags: [SFML, game, 2D, class]
---


How do computers handle the generation of random numbers? Let me explain with the help of Professor GPT.


> Computers generate random numbers using algorithms called **pseudo-random number generators** (PRNGs). These algorithms start with a seed value, typically based on some external input such as **the current time**, and then use mathematical operations to produce a sequence of numbers that appear random. However, since the process is deterministic, meaning that **given the same seed value, the sequence of numbers will be identical**, these numbers are not truly random. Instead, they are called pseudo-random because they mimic the behavior of true randomness for many practical purposes.
{: .prompt-info }


During the development of the Tetris game, I faced a similar issue: when dropping the block rapidly, identical next blocks were generated, and the pattern became recognizable. This made me realize that randomness should not be solely based on time.

<center>
<iframe src="https://drive.google.com/file/d/11JMuBTIyrOUx849ri8sQ7b3ou4LfDBvu/preview" width="480" height="640" allow="autoplay"></iframe>
	<br>
  <em>Figure 1. Tetris game with an inappropriate random algorithm</em>
</center>


Here is the code with the issue.
```cpp


  enum BlockType
  {
    T,
    I,
    S,
    Z,
    L,
    J,
    SQ,
    BlockTypeCount
  };
  
void TetrisBlock::createBlock(){	
    
		// Seed using the current time 
    std::srand(static_cast<unsigned int>(std::time(nullptr)));
    // Random BlockType using function rand()
    blockType = static_cast<BlockType>(std::rand() % BlockTypeCount);
    
    // more 
}
```
{: file='TetrisBlock::createBlock-error'}
<br>
<br>

This is how I fixed.
```cpp

void TetrisBlock::createBlock(){	
   
		// 1. Seed the Mersenne Twister pseudo-random number generator (mt19937) with a random value from std::random_device.
		std::mt19937 mt(std::random_device{}());
		// 2. Create a uniform distribution for generating integers between T and BlockTypeCount - 1, inclusive.
		std::uniform_int_distribution<int> dist(T, BlockTypeCount - 1);
		// 3. Generate a random integer using the distribution and the mt19937 generator, then cast it to a BlockType enum value.
		blockType = static_cast<BlockType>(dist(mt));
    
    // more 
}
```
{: file='TetrisBlock::createBlock-error'}


<center>
<iframe src="https://drive.google.com/file/d/11A2gFl2oR1alD_B2Sq1HrW1GFtaEpHWz/preview" width="480" height="640" allow="autoplay"></iframe>
	<br>
  <em>Figure 1. Tetris game with the Mersenne Twister pseudo-random number generator</em>
</center>



## Reference
[Wikipedia-Random number generation](https://en.wikipedia.org/wiki/Random_number_generation)<br>
[Wikipedia-Mersenne Twister pseudo-random number generator](https://en.wikipedia.org/wiki/Mersenne_Twister)<br>


