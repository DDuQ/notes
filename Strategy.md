# Definition

This pattern is used to pass different algorithms or behaviors to an object.

# When to use

A good rule of the thumb is when there is a class with a large number of conditional statements that switch between variants of the same algorithm. 

Algorithmic logic can be extracted into separate classes that implement the same interface. The context object then delegate the work to these classes, instead of implementing all the algorithms  itself.

# Pros and cons

|                                         **Pros**                                          |                                        **Cons**                                         |
| :-----------------------------------------------------------------------------------: | :---------------------------------------------------------------------------------: |
| Satisfies open/closed principle: can add new strategies without modifying the context | Clients have to be aware of the different algorithms and select the appropriate one |
|                 Can swap algorithms used inside an object at runtime                  | If there are just a couple of algorithms that rarely change it might be an overkill |

