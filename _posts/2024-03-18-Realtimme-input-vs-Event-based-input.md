---
title: Realtime input vs Event-based input
author: chulmin
date: 2024-03-18 00:00:00 -0500
categories: [SFML, Introduction]
tags: [SFML, game, 2D, input, realtime, event-based]
---

When it comes to videos and games, we encounter moving objects and sounds on a daily basis. However, the significant distinction between the two lies in the interaction between the player and the objects. In this post, we will explore the two types of input in games: real-time input and event-based input.


## Realtime input
Real-time input involves the continuous and immediate response of a game to the player's actions. The game constantly monitors the player's input and updates the game state accordingly. For example, when a player presses a button to move a character in a game, the character's position is instantly updated based on the duration and intensity of the button press. Real-time input is crucial in games that require quick reflexes and precise control, such as action or sports games.


## Event-based input
Event-based input, on the other hand, relies on discrete events triggered by the player's actions. Rather than continuous monitoring, the game waits for specific events to occur and responds accordingly. These events can include actions like button presses, mouse clicks, or touch gestures. When an event occurs, the game executes the associated logic or triggers predefined actions. Event-based input allows for more controlled and structured interactions, as the game can precisely handle each event individually. This type of input is commonly used in puzzle games, adventure games, or menu navigation.


## Implementation
Some parts in Game.hpp and Game.cpp are edited and added from the [previous post](https://chulminator.github.io/posts/SFML-using-class/).


```cpp

void Game::processEvents()
{
    sf::Event event;
    sf::Time deltaTime = clock.restart();  // Get the elapsed time since the last frame and restart the clock
    float dt = deltaTime.asSeconds(); // Convert the elapsed time to seconds

    while (mWindow.pollEvent(event))
    {
		handleEvent(event); // <- This line is inserted.

        if (event.type == sf::Event::Closed)
            mWindow.close();
    }
	handleRealtimeInput(); // <- This line is inserted.
}

void Game::handleEvent(const sf::Event& event) // <- this function is added
{
	if (event.type == sf::Event::KeyPressed)
	{
        if (event.key.code == sf::Keyboard::Up)
            printf("Up is pressed\n");
        else if (event.key.code == sf::Keyboard::Down)
            printf("Down is pressed\n");
        else if (event.key.code == sf::Keyboard::Left)
            printf("Left is pressed\n");
        else if (event.key.code == sf::Keyboard::Right)
            printf("Right is pressed\n");
	}
    
	else if (event.type == sf::Event::KeyReleased)
	{
        if (event.key.code == sf::Keyboard::Up)
            printf("Up is Released\n");
        else if (event.key.code == sf::Keyboard::Down)
            printf("Down is Released\n");
        else if (event.key.code == sf::Keyboard::Left)
            printf("Left is Released\n");
        else if (event.key.code == sf::Keyboard::Right)
            printf("Right is Released\n");
	}
}

void Game::handleRealtimeInput() // <- this function is added
{
    if (sf::Keyboard::isKeyPressed(sf::Keyboard::Up))
        printf("Real time Up\n");
    if (sf::Keyboard::isKeyPressed(sf::Keyboard::Down))
        printf("Real time Down\n");
    if (sf::Keyboard::isKeyPressed(sf::Keyboard::Left))
        printf("Real time Left\n");
    if (sf::Keyboard::isKeyPressed(sf::Keyboard::Right))
        printf("Real time Right\n");
}
```
{: file='Game.cpp'}

The game class now includes two additional functions: handleEvent and handleRealtimeInput. Despite these additions, the gameplay remains unchanged as the input is not directly linked to player movement. However, in the terminal, certain commands will show up. For instance, if I press the "up" key and release it within a second, a corresponding command will be

```bash
Up is pressed
Real time Up 
Real time Up
Real time Up
Real time Up
Real time Up
Real time Up
Up is Released
```
{: file='Terminal'}

And If I press the "down" and "left" keys simultaneously and then release them in the order of "down" followed by "left," a corresponding command will be

```bash
Down is pressed
Real time Down
Real time Down
Real time Down
Left is pressed
Real time Down
Real time Left
Real time Down
Real time Left
Real time Down
Real time Left
Down is Released
Real time Left
Real time Left
Left is Released
```
{: file='Terminal'}

## Conclusion
In this post, we delved into two distinct types of inputs: realtime input and event-based input. Realtime input involves responding to continuous movements within a game, while event-based input is triggered by discrete events. To illustrate this concept, let's consider the game Tetris. In Tetris, the actions of moving the block left, right, or down are examples of realtime input, as they require continuous input from the player. On the other hand, dropping the block to the floor is considered event-based input, as it is a specific action triggered by a discrete event. Understanding the differences between these inputs is crucial as it allows us to utilize them appropriately in different situations within a game.

## Reference 
[Haller, J. and Hansson, H.V., 2013. SFML Game Development. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development)<br>
[Pupius, R., 2015. SFML Game Development By Example. Packt Publishing Ltd.](https://www.packtpub.com/product/sfml-game-development-by-example)



