# Definition

Objects that help to create complex objects.
## When to use Factories

As a rule of thumb, it makes sense to use them only when we can simplify the code, otherwise object constructors should be enough.

Also take on mind to possible use cases:  
  

- When there is asynchronous logic involved in the creation of our object, since the constructor CANNOT handle asynchronism it will be a must to use a Factory.  

- If we want to use the result pattern (return either the object or a detailed error) we will require a Factory to return one of those values, which CANNOT be done by a constructor.

## Static Factory Methods  

Instead of creating a Factory, we can create a static method within our Domain Object and apply the same logic as if we were to create a Factory. In this way we can minimize the creation of simple Factories.

