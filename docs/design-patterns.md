---
title: Design Patterns
layout: default
---

# Design patterns

## Creational - creating objects

### Factory/Virtual constructor

- You have a Factory base class
- You have a ShipFactory and CarFactory child classes that return Vehicles
- Based on the situation, you can use a ShipFactory or a CarFactory as a Factory and return Vehicles which are either Cars or Ships

### Abstract Factory

- You have a Factory base class
- You have Chairs and Tables to sell but in 2 different designs: retro and modern. They are grouped by this category.
- You have a RetroFactory and ModernFactory child classes which have getChair() and getTable() methods.
- The difference from the Factory pattern is that with your sub factories you create a family of objects not just 1 type of object!

### Builder

- You have Cars that can be configured in a lot of ways.
- So you need Builders that do the specific work.
- Now you have a SuvBuilder and a SportsBuilder - both implement Builder.
- You may have (optional) a Director too. The director has a makeSuv(builder: Builder) and makeSports(build: Builder) methods. These methods call the correct methods in the right order on the Builder to build the specific type of Car.

### Singleton

- Make your constructor private so only the class can create an object
- Add a static private variable which holds a reference to the single instance
- Add a getter/method to the class: this getter will create the object only on the first time and after that it'll always return that instance.

### Prototype

- You want to clone an object - but you don't necessarily know the exact type of the object, only the interface it implements
- So you implement the clone() method on your class and make the class responsible for creating a new object and initialize the properties

## Structural - assemble objects

### Adapter/Wrapper

- You have XMLData and you want to use a lib that uses JSONData
- As they are not compatible, you need to write an XNLToJSONAdapter
- It accepts an XMLData as an input then it acts as a JSONData

### Composite/Object tree

- You have a webshop
- You have Boxes and Items
- Each Box can contain any number of Items and Boxes
- So you can have a tree-like structure: in the Boxes there are Boxes which also contain Boxes and Items
- So how do you calculate the sum of the price?
- Each Item returns its price. Each box returns the sum of its Items price plus the package price
- You need a common interface that defines e.g. the getPrice() method

### Decorator/Wrapper

- You have Car class
- You want to log every action some of the Car objects do
- As you don't want to add this feature to all of the Car objects you can't use inheritance, also you can't extend the functionalities of a class runtime
- So you create a CarLogger class that expects a Car as input and implements the same shape as the Car
- Your CarLogger should act as be used as a Car - you just add some logs and call the real Car object methods
- So it doesn't matter to the client code if it's a CarLogger because it behaves exactly like a Car

### Facade

- You downloaded a professional video editing package
- But your team only needs to create a simple 3 seconds video in 1 format
- You can create a facade class which exposes only the necessary functions of the video editing package to your app
- This way you can also be less dependent on the whole video editing package

## Behavioral - algorithms and object responsibilities

### Command/Transaction

- You have a save button next to a text area and a save icon in the toolbar
- Both components need to do the same - save the document
- Normally you would emit the event from your button then call the save() method in your business layer
- But instead you create a SaveCommand object and share it with both components
- SaveCommand has only 1 method: execute() - all other details are hidden in the command class
- SaveCommand will call your save() method in the business layer under the hood

### Observer

- Exactly like in rxjs
- You have a producer object that can emit a specific piece of information
- Subscriber object subject to the producer and they get notified when the given event occurs

### Strategy

- You have different solutions with the same interface for a problem
- Your Map has a reference to a NavigationStrategy which can be either a BikeNavigationStrategy or CarNavigationStrategy

### Iterator

- When you want to hide the underlying data structure you can use an Iterator interface that defines methods like next()
- e.g. You have an array but then want to use a map - by using them as objects implementing your Iterator you can swap them without causing any errors

### Mediator/Controller

- Like services in Angular
- Objects can communicate with each other through a Mediator object

### State

- Your app has different states like edit mode and read mode
- There is a big if-else in the code that changes the behavior of your class based on the active mode
- You extract these behaviors to EditState and ReadState objects
- Now your class can delegate all the state related work to these state objects after selecting the correct state object based on the mode

### Template method

- You have a complex algorithm in your base class
- Your subclasses could reuse most part of the logic - but they want to change some steps
- So the base class creates methods that can be overriden or implemented by the child classes
- These methods are called at specific steps during the complex algorithm
- This way the child classes can alter the behavior of the algorithm

## SOLID

# SOLID Principles

| Principle                                     | Description                                                                                                        | Example                                                                                            |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------------- |
| **S - Single Responsibility Principle (SRP)** | A class should have one and only one reason to change, meaning it should only have one job.                        | A `User` class should handle user data, but authentication logic should be in an `Auth` class.     |
| **O - Open/Closed Principle (OCP)**           | Software entities should be open for extension but closed for modification.                                        | Use interfaces or inheritance to add new functionality without changing existing code.             |
| **L - Liskov Substitution Principle (LSP)**   | Objects of a superclass should be replaceable with objects of a subclass without affecting the program's behavior. | A `Rectangle` class should work when replaced with a `Square` subclass without breaking the logic. |
| **I - Interface Segregation Principle (ISP)** | A client should not be forced to implement an interface it does not use.                                           | Create smaller, specific interfaces instead of large, general ones.                                |
| **D - Dependency Inversion Principle (DIP)**  | High-level modules should not depend on low-level modules. Both should depend on abstractions.                     | Use dependency injection to provide dependencies rather than creating them internally.             |
