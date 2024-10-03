---
title: SFML with Class structure
author: chulmin
date: 2024-03-11 00:00:00 -0500
categories: [Game, SFML, Introduction]
tags: [SFML, game, 2D, class]
---

This post is specifically targeting Windows 10 users.

## Advatange of stucturing with class

In this post, we are going to do the same thing as we did in [Hello SFML](https://chulminator.github.io/posts/Hello_SFML/), but with a twist - we'll be using classes. Although it may seem tricky at first, utilizing classes offers several advantages:

1. Modularity and Structuring:
  - Classes allow for better modularity and structuring of code. Each class represents a specific functionality or concept, reducing dependencies between different parts of the code.
  - This enhances readability and makes maintenance easier.

2. Reusability:
  - Classes facilitate the creation of multiple objects with similar functionalities, increasing code reusability and reducing development time.
  - Classes can be reused in other projects or modules.

3. Inheritance and Polymorphism:
  - Classes support inheritance, enabling the extension and specialization of existing classes. Polymorphism allows objects to be treated uniformly despite differences in their implementations.
  - This enhances code flexibility and reduces redundancy.

4. Ease of Debugging and Testing:
  - Classes break down code into smaller units, making debugging and testing easier. Each class represents a specific functionality, simplifying testing for that functionality.
  - This increases code reliability and makes it easier to identify and fix bugs.

## Implementation 
Since the code is simple, initially, we'll create a single class named Game, which will be added in Game.hpp and Game.cpp files. However, as the game or program becomes more complex, it may require the creation of additional classes to handle various aspects of the game logic. For example, the Player entity could be detached from the Game class and implemented as a separate class, utilizing relevant variables such as position and velocity.

One thing that differs from [Hello SFML](https://chulminator.github.io/posts/Hello_SFML/) is the adoption of TimePerFrame. By incorporating TimePerFrame, we ensure a constant number of frames being executed, which is crucial for maintaining stability and providing a consistent user experience. This feature helps prevent the game from running too fast or too slow on different hardware setups, providing a smoother and more enjoyable gameplay experience for all users.


```cpp
#include "./include/Game.hpp"

int main()
{
    Game game;  // Create a Game object.
    game.run(); // Run the game.
}
```
{: file='main.cpp'}




```cpp
#pragma once

#include <SFML/Graphics.hpp>
#include <cassert>

class Game
{
public:
    Game();     // Constructor
    void run(); // Run the game

private:
    void processEvents(); // Process events such as keyboard input
    void update();        // Update the game logic
    void render();        // Render the game objects

private:
    sf::RenderWindow mWindow;   // SFML window for rendering
    sf::Text mPlayer;           // Text object for the player
    sf::Font mFont;             // Font object for text rendering
    sf::Vector2f position;      // Player position
    sf::Vector2f velocity;      // Player velocity
    sf::Clock clock;            // Clock for measuring time
    sf::Time TimePerFrame;      // Time per frame
};

```
{: file='./include/Game.hpp'}



```cpp
#include "Game.hpp"

Game::Game():
  mWindow(sf::VideoMode(640, 480), "SFML Hello, World!"),
  mFont(),
  TimePerFrame( sf::seconds(1.f/60.f)  ),
  position(100.f, 100.f),
  velocity(200.f, 200.f),
  mPlayer()
  {
    if (!mFont.loadFromFile("./Media/TIMES.ttf"))  // Load the font
    {
        assert(false);
    }
    mPlayer.setString("Hello, Chulminator!");  // Set the text content
    mPlayer.setFont(mFont);                    // Set the font for the text
    mPlayer.setCharacterSize(30);              // Set the character size of the text
    mPlayer.setFillColor(sf::Color::White);    // Set the fill color of the text
  }

void Game::run()
{
	sf::Time timeSinceLastUpdate = sf::Time::Zero;
  while (mWindow.isOpen())
  {
    sf::Time elapsedTime = clock.restart();
    timeSinceLastUpdate += elapsedTime;
    while (timeSinceLastUpdate > TimePerFrame){    
      timeSinceLastUpdate -= TimePerFrame;
      processEvents();  // Process events
      update();         // Update game logic
    }
    render();           // Render the game
  }
}

void Game::processEvents()
{
  sf::Event event;
  sf::Time deltaTime = clock.restart();  // Get the elapsed time since the last frame and restart the clock
  float dt = deltaTime.asSeconds(); // Convert the elapsed time to seconds

  while (mWindow.pollEvent(event))
  {
      if (event.type == sf::Event::Closed)
          mWindow.close();
  }
}

void Game::update()
{
  position += velocity * TimePerFrame.asSeconds(); // Update the position of the text based on its velocity and the elapsed time
  // In case the collision with the wall 
  if (position.x + mPlayer.getGlobalBounds().width >= mWindow.getSize().x || position.x <= 0)
  {
      velocity.x = -velocity.x;  // Reverse the horizontal velocity
  }
  if (position.y + mPlayer.getGlobalBounds().height >= mWindow.getSize().y || position.y <= 0)
  {
      velocity.y = -velocity.y;  // Reverse the vertical velocity
  }
  mPlayer.setPosition(position); // Set the new position of the text
}

void Game::render()
{
    mWindow.clear();        // Clear the window
    mWindow.draw(mPlayer);  // Draw the text
    mWindow.display();      // Display the rendered frame
}

```
{: file='./include/Game.cpp'}

```
├── include
│   ├── Game.hpp
│   └── Game.cpp
├── Media
│   ├── TIMES.TTF
├── src
│   └── lib
│       └── *.a
├── main.cpp
├── Makefile
└── *.dll
```
{: file='Folder structure'}


```shell
CC = g++
TARGET = my_program # Build Target
LIB_DIR = src/lib/ # SFML Library Directory
INCLUDE_DIR = C:\SFML\include # SFML Header File Directory
SRCINCLUDE_DIR = include\

CFLAGS = -I$(INCLUDE_DIR)
LDFLAGS = -L$(LIB_DIR)
LIBS = -lsfml-graphics -lsfml-window -lsfml-system -lopengl32 -lsfml-audio

# Source File List
SRCINCLUDE = Game.cpp
SRCINCLUDE := $(addprefix $(SRCINCLUDE_DIR), $(SRCINCLUDE))
SRCS = main.cpp $(SRCINCLUDE)

# Object File List (Generated by compiling source files)
OBJS = $(SRCS:.cpp=.o)

# Build Rules
$(TARGET): $(OBJS)
$(CC) $(LDFLAGS) -o $@ $(OBJS) $(LIBS)

%.o: %.cpp
$(CC) $(CFLAGS) -c $< -o $@

# Clean Target
clean:
del /F $(OBJS) $(TARGET)
```
{: file='Makefile'}

## Reference
[Haller, J. and Hansson, H.V., 2013. SFML Game Development. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development)<br>
[Pupius, R., 2015. SFML Game Development By Example. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development-by-example)

