---
title: Design Patterns
layout: default
---

# Design patterns

## Creational - creating objects

### Factory

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

## Structural - assemble objects

### Adapter

- You have XMLData and you want to use a lib that uses JSONData
- As they are not compatible, you need to write an XNLToJSONAdapter
- It accepts an XMLData as an input then it acts as a JSONData

### Composite

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
