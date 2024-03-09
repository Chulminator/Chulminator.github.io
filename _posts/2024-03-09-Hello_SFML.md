---
title: Hello SFML
author: chulmin
date: 2024-03-09 00:00:00 -0500
categories: [SFML]
tags: [SFML, game, 2D, development, compile, hello world]
---

This guide is specifically for Windows 10 users.

## What is SFML?

SFML, which stands for Simple and Fast Multimedia Library, is a free and open-source multimedia library that is primarily used for developing applications with 2D graphics. It is written in C++ and offers a wide range of features for multimedia development, including graphics, audio, and networking.

SFML provides developers with an intuitive and easy-to-use API (Application Programming Interface) that allows them to manipulate graphics, handle audio playback, and implement network communication with convenience. It is designed to be platform-independent, meaning it can be used on various operating systems such as Windows, macOS, and Linux.

In terms of graphics, SFML supports 2D graphics and provides functions for tasks like creating windows, drawing shapes and sprites, handling animations, and processing events. This makes it suitable for developing 2D games and applications that require graphics rendering.

SFML is not only useful for game development but also for creating multimedia applications in general. It provides a range of example code and detailed documentation, making it accessible to developers of all levels, including beginners. The library is actively maintained and improved by a dedicated community of developers, ensuring its stability and compatibility across different platforms.

## Hello SFML
Let's begin by creating the simplest example. We'll start by displaying "Hello, Chulminator!" on the screen. First, we'll create a window to serve as our game space. For the font, let's use "Times New Roman". Make sure to place the *.ttf file in the appropriate directory. Next, we'll define a text object that displays "Hello, Chulminator!" using the specified font and size. The text will move around the screen, and when it reaches the edges, it will bounce off the walls. The remaining code involves some technical processes.

```cpp
#include <SFML/Graphics.hpp>

int main()
{
    sf::RenderWindow window(sf::VideoMode(800, 600), "SFML Hello, World!");
    // Create a window: the window size is 800 by 600 pixes.
    // And SFML Hello, World! will be printed on the window

    sf::Font font;
    if (!font.loadFromFile("./Media/TIMES.ttf"))  // Load the font
    {
        return EXIT_FAILURE; // Handle font loading failure
    }

    sf::Text text("Hello, Chulminator!", font, 30); // Create the text
    text.setFillColor(sf::Color::White); // Set the text color

    sf::Vector2f position(100.f, 100.f); // Set the initial position of the text
    sf::Vector2f velocity(200.f, 200.f); // Set the initial velocity of the text

    sf::Clock clock; // Create a clock to measure the elapsed time

    while (window.isOpen()) // Start the game loop, which runs as long as the window is open
    {
        sf::Time deltaTime = clock.restart();  // Get the elapsed time since the last frame and restart the clock
        float dt = deltaTime.asSeconds(); // Convert the elapsed time to seconds

        sf::Event event;
        while (window.pollEvent(event))  // Check for events happening in the window
        {
            if (event.type == sf::Event::Closed)  // In case, the window is closed
                window.close();
        }

        position += velocity * dt; // Update the position of the text based on its velocity and the elapsed time

        // In case the collision with the wall 
        if (position.x + text.getGlobalBounds().width >= window.getSize().x || position.x <= 0)
        {
            velocity.x = -velocity.x;
        }
        if (position.y + text.getGlobalBounds().height >= window.getSize().y || position.y <= 0)
        {
            velocity.y = -velocity.y;
        }

        text.setPosition(position); // Set the new position of the text

        window.clear(); // Clear the window
        window.draw(text); // Draw the text on the window
        window.display(); // Display the updated window content
    }

    return 0;
}
```
{: file='Hello world'}


## compiling SFML

There are several ways to compile the code, and I use a Makefile for that purpose. It's important to note that you should specify the exact SFML Header file directory in this Makefile, and some *.dll files should be in the same directory as the Makefile. As the code becomes more complex, you can create an "include" directory and place source and header files there.

To compile the files, you can use the command 'MinGW32-make'. To remove the generated object files, you can use the command 'MinGW32-make clean'. Here, MinGW is a Windows utility, short for Minimalist GNU for Windows. Below is an example of the folder structure and the corresponding Makefile:


```
├── include
├── Media
│   ├── TIMES.TTF
├── Media
│   └── src
│       └── lib
│           └── *.a
├── main.cpp
├── Makefile
├── openal32.dll
├── sfml-audio-2.dll
├── sfml-audio-d-2.dll
├── sfml-graphics-2.dll
├── sfml-graphics-d-2.dll
├── sfml-network-2.dll
├── sfml-network-d-2.dll
├── sfml-system-2.dll
├── sfml-system-d-2.dll 
├── sfml-window-2.dll
└── sfml-window-d-2.dll
```
{: file='Folder structure'}

and

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
SRCINCLUDE =
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


## Conclusion
<p align = "center">
<iframe src="https://drive.google.com/file/d/1c-M92F5xXkGg-DiEq5_ZGoVKKzf6OFDV/preview" width="360" height="270" allow="autoplay"></iframe>
  <br>
  <em>Figure 1. Result</em>
</p>
<!-- *Figure 1. Result* -->
<!-- <iframe src="https://drive.google.com/file/d/1psJ7MXSl1dn1xoTxNTIV2gLRQnkVk3Dj/preview" width="640" height="480" allow="autoplay"></iframe> -->



This marks the start of the SFML post. The goal is to understand the basic structure of SFML code and enable code execution. After successfully running the code, experimenting with various settings within the code is recommended. Additionally, if anyone has experience compiling SFML code using VS Code, suggestions to improve this post are welcomed.



## Reference
[Haller, J. and Hansson, H.V., 2013. SFML Game Development. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development)<br>
[Pupius, R., 2015. SFML Game Development By Example. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development-by-example)

