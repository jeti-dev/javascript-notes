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

## Behavioral - algorithms and object responsibilities
