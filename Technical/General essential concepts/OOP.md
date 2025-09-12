# Definition

OOP stands for Object-Oriented Programming. 

It’s a paradigm where we model software around objects, which represent real-world or abstract entities. Objects are created from classes and can have properties (to hold state) and methods (to define behavior).

# Core principles

## Encapsulation

Involves bundling properties and methods (behaviors) into a single unit called a class.
Helps hiding internal logic of an object, by exposing only necessary methods.

## Abstraction

Helps reducing complexity by hiding unnecessary details.

## Inheritance

Involves creating new classes (**derived**) based on existing classes (**base**).

Derived classes *will inherit properties and methods* from their base classes. Also, it can add new features or override existing ones.

Inheritances usually is described in terms of **"is-a"** relationship.

## Polymorphism

Ability of an object to take many forms.

## Coupling

Degree of dependency between different classes. 

There are two types:
### Tight coupling 
Class A depends directly on class B.
### Loose coupling
Class A depends on an abstraction Interface X. 

## Composition

Involves creating complex objects by combining simpler objects or components. 

Objects are assembled together to form larger structures, with each component maintaining its own state and behavior.

Composition is often described in terms of **"has-a"** relationship.

## Composition VS Inheritance

### When to use composition
- Whenever flexibility is needed in constructing complex objects by assembling smaller, reusable components.

- When there is no clear **"is-a"** relationship between classes, and a **"has-a"** relationship fits better.

- To avoid limitations of inheritance; tight coupling and the [[OOP#Fragile Base Class Problem|fragile base class problem]].

### When to use inheritance
- When there is a clear **"is-a"** relationship between classes, and derived classes can be treated as instance of their base class.

- When we want to leverage [[OOP#Polymorphism|polymorphism]] to allow objects of different derived classes to be treated uniformly through their common base class.

```csharp
	var vehicles = new List<Vehicles>();
	vehicles.Add(new Car {NumberOfDoors = 4, Brand = "Toyota"});
	vehicles.Add(new Motorcycle {Brand = "Harley Davidson"});
	
	//Vehicle inspection
	foreach(var vehicle in vehicles)
	{
		vehicle.Start();
	}
	```

- To promote code reuse by inheriting properties and behaviors from existing classes.


# Fragile Base Class Problem

Software design issue that arises in object-oriented programming when changes made to a base class can inadvertently break the functionality of derived classes. 

This occurs due to [[OOP#Tight coupling|tight coupling]] between base and derived classes in inheritance hierarchies.
## Inheritance coupling
Inheritance creates a strong coupling between the base class and derived classes. Any changes can potentially affect the behavior of all derived classes.
## Limited extensibility
Modifications to the base class can become increasingly risky and costly over time. Developers avoid making necessary changes due to the fear of breaking existing functionality. (This is known Brittle software)
## **Mitigation strategies**
Design principles such as Open/Closed and Dependency Inversion can be used, as well as design patterns like Composition over inheritance.

These approaches promote loose coupling, encapsulation and modular design reducing the impact of changes in base classes.


# Source of concepts

[Master Design Patterns & SOLID Principles in C# - Full OOP Course for Beginners](https://www.youtube.com/watch?v=rylaiB2uH2A&t)
