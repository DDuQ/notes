# Definition

Allows us to define a family of algorithms, encapsulate each one and make them **interchangeable** at runtime.

Instead of hardcoding behavior in a class, it can delegate behavior to an 'strategy' object, which will be switch out as required by high-level classes.

# Structure

## Strategy (interface/abstraction)
Defines a common behavior contract.
## Concrete Strategies
Different implementations of the algorithm.
## Context (high-level class)
Uses a [[Technical/Essential Concepts/Design Patterns/Behavioral/Strategy#Strategy (interface/abstraction)|strategy]] object to perform a task.

# When to use

Whenever there are several conditional statements that switch between variants of the same algorithm.

The algorithm logic can be extracted into separate classes that implement the same interface.

# Pros and Cons

| Pros                                                                                      | Cons                                                                                        |
| :---------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------ |
| <br>Satisfies Open/Closed principle: can add strategies without modifying the context<br> | <br>Clients have to be aware of the different algorithm and select the appropriate one<br>  |
| <br>Can swap algorithms used inside an object at runtime                                  | <br>If you only have a few algorithms that rarely change, then this may be over-engineering |
