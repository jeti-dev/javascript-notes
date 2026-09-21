---
title: Python
layout: default
---

```python

# comment
print("hello")
print("hello", end=" ")


## Basic types

first_name = "John"
age = 30
price = 10.99
is_student = False

print(f"Hello, {first_name}!")


## Type casting

type(first_name)  # <class 'str'>
bool(first_name)  # True
str()
int()
float()
bool()


## input()
# name = input("Enter your name: ")

## if-else
if age < 18:
    print("You are a minor.")
elif age < 65:
    print("You are an adult.")
else:
    print("You are a senior citizen.")

## Logical operators: and, or, not
# Example of logical operators

## Conditional expressions
age_group = "Adult" if age >= 18 else "Minor"

## String methods
len(first_name)
first_name.find("o")
first_name.rfind("o")
first_name.isdigit()
first_name.isalpha()
first_name.count("o")
first_name.replace("o", "i")

credit_number = "1234-5678-9012-3456"
credit_number[0:4]  # First 4 digits
credit_number[-4:]  # Last 4 digits
credit_number[::3] # Every third character
credit_number[::-1]  # Reverse the string

## Format specifiers
price1 = 3.123456
print(f"Price1: {price1:.2f}")  # 3.12
print(f"Price1: {price1:10}") # 3.12 (right-aligned in a field of width 10)

## While loop
count = 0
while count < 5:
    print(f"Count is {count}")
    count += 1

## For loop
for i in range(1, 11):
    print(f"Iteration {i}")

# reversed
for i in reversed(range(1, 11)):
    print(f"Iteration {i}")

# step 3
for i in reversed(range(1, 11, 3)):
    print(f"Iteration {i}")

# loop string
for x in credit_number:
    print(x)


## Collections

# List - ordered, mutable, allows duplicates
fruits = ["apple", "banana", "cherry"]

if "apple" in fruits:
    print("Apple is in the list.")

fruits.remove("banana")
fruits.count("apple")

# Set - unordered, immutable, no duplicates
fruits_set = {"apple", "banana", "cherry"}
# fruits_set[0] # Error: Sets are unordered, so indexing is not allowed

# Tuple - ordered, immutable, allows duplicates
fruits_tuple = ("apple", "banana", "cherry")

# Dictionary - key-value pairs, ordered, mutable
fruits_dict = {
    "apple": 1,
    "banana": 2,
    "cherry": 3
}

if fruits_dict.get("France") == None:
    print("France is not in the dictionary.")

fruit_items = fruits_dict.items()  # Returns a view object that displays a list of a dictionary's key-value tuple pairs

for key, value in fruit_items:
    print(f"{key}: {value}")

# 2d list - works with tuples and lists
matrix = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]


## Random numbers
import random
# Generate a random integer between 1 and 10
random_integer = random.randint(1, 10)
# Generate a random float between 0 and 1
random_float = random.random()
# Generate a random choice from a list
choices = ["apple", "banana", "cherry"]
random_choice = random.choice(choices)
# Shuffle a list randomly
random.shuffle(choices)
# Generate a random sample of 2 elements from a list
random_sample = random.sample(choices, 2)
# Generate a random float between 1 and 10
random_uniform = random.uniform(1, 10)


## Functions
def greet(name):
    """Function to greet a person."""
    print(f"Hello, {name}!")

greet("John")


## Default arguments
def greet(name="Guest"):
    """Function to greet a person with a default name."""
    print(f"Hello, {name}!")
greet()


## Keyword arguments
def greet(name, age):
    """Function to greet a person with a default name and age."""
    print(f"Hello, {name}! You are {age} years old.")
greet(age=25, name="Alice")


## Arbitrary arguments
def greet(*args): # args is a tuple
    """Function to greet multiple people."""
    for name in args:
        print(f"Hello, {name}!")
greet("Alice", "Bob", "Charlie")

# Arbitrary keyword arguments
def greet(**kwargs): # kwargs will be a dictionary from the arguments
    """Function to greet multiple people with their ages."""
    for name, age in kwargs.items():
        print(f"Hello, {name}! You are {age} years old.")
greet(Alice=25, Bob=30, Charlie=35)

# * = unpacking operator


## Memberhsip operators
"a" in "apple"
"b" not in "banana"


## List comprehensions - concise way to create lists
doubles = [x*2 for x in range(10) if x % 2 == 0]
print(doubles)


## Match-case statement
day = "monday"

match day:
    case "monday" | "tuesday":
        print("it's weekday")
    case _:
        print("unknown day")


## Modules
# import math
# import math as m
# from math import pi
# You create a new file, put stuff in there. The file name will be the module name.


## Variable scope
# LEGB: local -> enclosed -> global -> built-in


##__main__

def main():
    print("Start the app")

if __name__ == "__main__":
    main()

# When the script file is run directly, it will be the "main" module. Otherwise __name__ is the filename.
# e.g. create a library that shows a help page only when run directly and not when imported



## OOP
class Car:
    def __init__(self, model, year):
        self.model = model
        self.year = year

    def drive(self):
        print("f{self.model} is driving")

car1 = Car("Toyota", 2020)

## Class variables = static variable
class Student:

    class_year = 2024
    num_students = 0

    def __init__(self, name):
        self.name = name
        Student.num_students += 1

Student.class_year


## Inheritance
class Animal:
    def __init__(self, name):
        self.name = name

    def eat(self):
        print(f"${self.name} is eating")

class Dog(Animal):
    def speak(self):
        print("woof")

dog1 = Dog("Scooby")

## Multiple inheritance and multilevel inheritance

class Animal2:
    def __init__(self, name): # constructor is passed down
        self.name = name

class Prey(Animal2):
    def flee(self):
        print(f"{self.name} flee")


class Predator(Animal2):
    def hunt(self):
        print(f"{self.name} hunt")

class Fish(Prey, Predator):
    pass

fish1 = Fish("nemo")
fish1.flee()

## super() to call methods from parent
# in the constructor: super().__init__(color, radius)


## Polymorphism
from abc import ABC, abstractmethod

class Shape(ABC):

    @abstractmethod
    def area(self):
        pass

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius

    def area(self):
        return self.radius * 2

circle1 = Circle(2)
circle1.area()


## Duck typing - the object has the minimum requirement for a shape


## Static methods
class Employee:

    @staticmethod
    def is_valid_position():
        return False


## Class methods
class Student:
    count = 0

    @classmethod
    def get_count(cls):
        return f"total: {cls.count}"


## Magic methods = dunder methods
# __init__, __str__, __eq__, __lt__, __gt__, __add__, __contains__


## property = getter/setter decorator
class Rectangle:
    def __init__(self, width, height):
        self._width = width
        self._height = height

    @property
    def width(self):
        return f"{self.width}cm"

    @width.setter
    def width(self, val):
        self.width = val


## Decorator
def add_sprinkles(func):
    def wrapper(*args, **kwargs):
        print("Add sprinkles")
        func(*args, **kwargs)
    return wrapper

@add_sprinkles
def get_ice_cream(flavor):
    print(f"Here is your ice cream {flavor}")


## Exception
try:
    print("hi")
except ZeroDivisionError:
    print("ZeroDivisionError")
except TypeError:
    print("TypeError")
except Exception:
    print("Catch everything")
finally:
    print("finally")


## Multithreading
import threading
import time

def walk_dog(name):
    time.sleep(8)
    print(f"walk dog {name}")

chore1 = threading.Thread(target=walk_dog, args=("Scooby", ))
chore1.start()

chore1.join() # waits to finish

print("after thread finished")
```
