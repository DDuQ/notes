## Identity Uniqueness  

Helps us to differentiate between Aggregates.

- An Aggregate’s identifier must be unique within the system.
- An entity’s identifier must be unique within its parent aggregate.

>[!NOTE]
If we are using GUIDs within our system it is safe to say that an Entity is unique even without checking its parent identifier. But if the IDs are auto-incrementing within its parent aggregate the bullets mentioned should be considered.

## Domain object deletion  

We should be aware that any change we make within an aggregate SHOULD NOT affect other aggregates in our databases. It should be handled consistently via domain events.

|                              |     |
| ---------------------------- | --- |
| Changes via DB relationships | ❌   |
| Changes via Domain Events    | ✅   |
