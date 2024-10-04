---
title: Godot - Syntax cheat sheet
author: chulmin
date: 2024-10-03 00:00:00 -0500
categories: [Game, Godot]
tags: [Game, Godot, syntax]
math: true
---

## Getting Started with Godot: A GDScript Cheat Sheet

I recently started exploring Godot, and I've found that the syntax of GDScript is quite similar to Python. I asked ChatGPT for a cheat sheet, which I’m sharing here for my future self—who tends to forget things easily.

I haven’t tested everything yet, so if I come across any incorrect syntax in this article, I’ll update it accordingly.

## Variables and Constants
```gdscript
var my_variable = 10  # Variable declaration
const MY_CONSTANT = 100  # Constant declaration

# Type hinting
var my_int: int = 42
var my_string: String = "Hello, Godot"
```

## Functions
```gdscript
func my_function():
    print("Hello, Godot")

# Parameter and return type specification
func add_numbers(a: int, b: int) -> int:
    return a + b
```

## Conditionals
```gdscript
if condition:
    print("Condition is true")
elif other_condition:
    print("Another condition")
else:
    print("Condition is false")
```

## Loops
```gdscript
# For loop
for i in range(5):
    print(i)  # Outputs: 0, 1, 2, 3, 4

# While loop
var count = 0
while count < 5:
    print(count)
    count += 1
```

## Arrays and Dictionaries
```gdscript
# Array
var my_array = [1, 2, 3, 4]
my_array.append(5)  # Add an element

# Dictionary
var my_dict = {
    "name": "Godot",
    "version": 4
}
print(my_dict["name"])  # Outputs: "Godot"
```

## Classes and Inheritance
```gdscript
extends Node2D  # Parent class

# Class variable and function
class_name MyNode

var my_value: int = 0

func _ready():
    print("Node is ready")
```

## Signals
```gdscript
# Define a signal
signal my_signal(value)

# Connect the signal
func _ready():
    connect("my_signal", self, "_on_my_signal")

# Emit the signal
emit_signal("my_signal", 42)

# Signal callback function
func _on_my_signal(value):
    print("Signal received with value: ", value)
```

## Node References
```gdscript
# Reference a node by path
onready var my_node = $NodeName

# Find a child node by path
var child_node = get_node("ChildNodeName")
```

## Scene Transitions
```gdscript
# Change scene
get_tree().change_scene("res://scenes/next_scene.tscn")

# Load and instance a scene resource
var scene = load("res://scenes/my_scene.tscn")
var instance = scene.instance()
add_child(instance)
```

## User Input
```gdscript
func _input(event):
    if event.is_action_pressed("ui_accept"):
        print("Accept button pressed")
```

## Playing Audio
```gdscript
# Play AudioStreamPlayer
$AudioStreamPlayer.play()
```

## Timer
```gdscript
# Timer setup
func _ready():
    $Timer.start()

# Timer signal connection
func _on_Timer_timeout():
    print("Timer timed out!")
```

## Random Numbers
```gdscript
var rand_value = randi() % 100  # Random value between 0 and 99
```

## Physics Process
```gdscript
func _physics_process(delta):
    # Called every physics frame
    position.x += 100 * delta
```

## GUI Control (Control Node)
```gdscript
# Change text label
$Label.text = "New Text"

# Connect button click event
func _on_Button_pressed():
    print("Button pressed!")
```