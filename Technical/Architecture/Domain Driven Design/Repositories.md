# Definition

An illusion of manipulation of Entities in memory, it abstracts us from the persistence management.

## Who uses it?

- Application and Domain Services.

Repositories SHOULD only be used through Aggregate Roots, since as stated before, those ensure the integrity of the aggregate.
