## Prefer smaller Aggregates 

- Less data to fetch from the database.
- Lower chance of race conditions (2 changes happen at the same time, we need to fail one request).

## One aggregate per transaction  

Related to the previous rule, we want to use eventual consistency outside the boundary.

Here we isolate one transaction to the process of deleting a Room, while wrapping another one to do subsequent transactions for removals/deletions via Domain Events.

## Reference between domain models 

- Aggregates ONLY reference other aggregates by Id.

Note: As a common pattern, strongly typed Ids are used to encapsulate GUIDs (meaning creating a Value Object that contains this kind of Ids).

## No referencing nested entities

- An Id of an entity is only unique within its parent aggregate, implying that if we were to reference a nested entity within another aggregate it might not be unique within that entire system.