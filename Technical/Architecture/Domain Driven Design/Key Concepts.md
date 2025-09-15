# Orchestration approach  

Every change that takes place in certain handler is in charge to orchestrate all the changes and all the side effects to all the various aggregates that are part of that request.

# Eventual Consistency 

Processing is divided. Model in which we distribute the logic to be propagated.  
  
# Domain Events  

Something interesting from a business point of view, that happened within the system. 

- Every event should be handled one at a time and our system should be resilient enough in case of failed events.

# Bounded Contexts

A logical boundary in which a domain model applies.

- Unlike sub-domains, it is more flexible, we can have a domain object that applies for many bounded contexts.