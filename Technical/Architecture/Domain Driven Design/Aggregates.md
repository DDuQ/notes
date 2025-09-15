# Definition 

One or more domain objects that always need to stay consistent as a whole. Changes should be as atomic as possible.

- Consistency means to give a set of entities/value objects a meaning within an established boundary.  
  An example may be an order and its line-items, these will be separate objects, but it's useful to treat an order (together with its line items) as a single aggregate.

Aggregates are the basic element of transfer of data storage - you request to load or save whole aggregates. Transactions should not cross aggregate boundaries.

## Aggregate root  

It always refers to a specific entity that is key within the boundary. 

An aggregate will have one of its component objects be the aggregate root. Any references from outside the aggregate should only go to the aggregate root. The root can thus ensure the integrity of the aggregate as a whole.

Examples of how Aggregates, Entities, Value objects can co-exist within a boundary:

  ![[DDD (Domain Driven Design)-1.jpeg|315x254]]![[image.jpeg|314x254]]

![[DDD (Domain Driven Design)-2.jpeg|640x340]]


Example of an Aggregate root (session):  

![[DDD (Domain Driven Design)-3.jpeg]]
