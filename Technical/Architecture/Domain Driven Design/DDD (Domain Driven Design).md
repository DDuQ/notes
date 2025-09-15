#  Concept

Approach to break down the complexities of business rules, that will be modeled through a code base/architecture.

# Domain  

Area of knowledge or very specific and specialized activity around which the application logic revolves.

# The need for DDD  

To bridge the gap between business and code. By leveraging the use of Ubiquitous Language, Invariants and Objects of the domain.

DDD is split into **strategic** and **tactical** design.

# Strategic Design (What/Why?) 

Visualize the whole picture of our Domain, being able to see the relationships between sub-domains.

# Tactical Design (How?) 

The way that a specific domain is composed and all the parts involved (code-side). Codebase that represents the Domain.

## Tactical Patterns 

Set of building blocks that allow us to model our Domain in a way that resembles the problem space.

>[!NOTE] Noteworthy Patterns: 
 Modules 
 Application Services 
 Bounded Contexts 
 Ubiquitous Language

## Tactical Design vs Tactical Patterns 

With Tactical Design we model our Domain, and we build it through the tools that Tactical Patterns provides.

# (Tactical vs Strategic) Design 

Both are complementary in an iterative process of discovering business rules in the process and to model them. Both Tactical and Strategic Design compose DDD.
 
# Application Services vs Domain Services

## Application Services  

Responsible for orchestrating domain objects like Aggregates, Entities, Value Objects but also other tactical patterns like Repositories, Factories, etc.

## Domain Services 

Business logic (decisions) that cannot be handled within a domain object.

> [!Quote]
> “Domain logic is everything that is related to business decisions, Domain services, therefore, participate in the decision-making process the same way entities and value objects do. And application services orchestrate those decisions the same way they orchestrate decisions made by entities and value objects.” 
> 
> \- Vladimir Khorikov
## What counts as domain logic?

- Whenever we are deciding on the experience of the user it is most likely domain logic.
- Dependence on some state (A happens then B), magic numbers.


# Transactional Consistency vs Eventual Consistency

## Transactional Consistency 

When the entire processing needs to happen as a whole (all or nothing approach), either everything succeeds, or anything succeeds.

## Eventual Consistency 

Processing is broken down into smaller chunks and the entire state of the processing stays consistent.

### Use eventual consistency outside the boundary

- DDD enables us to take complex business into small atomic units called aggregates, which have everything they need to make a business decision, giving us the confidence to make changes to that specific component and propagating events as needed to other aggregates, that will change based on their own business decisions.
    

# Rich vs Anemic domain Models

- Both are OOP approaches for designing domain models.

## Anemic Domain Model

1. All/most properties have public getters/setters.
2. All/most properties have no logic about that domain model.
3. Decision around manipulating properties is not done by the object.

## Rich Domain Model

1. Object that conceals the underlying data.
2. Exposes only a set of specific methods that allow the interaction with underlying data.

### Rich Domain Modeling Guidelines

- Private fields and properties by default.
- Expose only when needed.
- Expose only what’s needed.
- Notice “Primitive Obsession” (consider extracting Value Objects → Location).
- Notice use of Domain Models in Application Services.

# Always valid vs Not always valid [[Domain Modeling|domain models]]

## Always Valid 

No matter when, the data is always in a valid state, their properties are always in a valid state.

## Not Always Valid 

Approach where we allow the domain object to be in an invalid domain state (validation errors).

>[!NOTE]
Prefer Always Valid Domain Model → We can’t access a property that is in an invalid state. We could use Factory pattern or static factory method for validation purposes. 

## How to always maintain our domain valid?

### Make the boundary of your domain model explicit

- It should be clear which classes in your code base represent the domain model and which do not.
### Validate all external requests before they cross that boundary

- You need to perform all required validations upfront, before you delegate the work to domain classes.


Here’s an image provided by Vladimir Khorikov that pictures this idea on how to approach [Always Valid domain models](https://enterprisecraftsmanship.com/posts/always-valid-vs-not-always-valid-domain-model/):

![[DDD (Domain Driven Design)-4.jpeg]]

## Persistence ignorance 

We want to focus only on the modeling part of DDD considering only the domain (business rules, decisions, terminology and objects).

- Repository pattern promotes this by abstracting ourselves on the way objects are going to be persisted.

  

# Strategic Design

Process of defining the large-scale structure of the system and mapping out the business domain.

How to break up the domain relationships into chunks.

## How is this done?

### Domain Experts 
 Leverage their experience to approach the problem space (Ubiquitous language).

### Event Storming 
Getting all the decision-makers in one room, using sticky notes to visualize the timeline and some of the issues there might be.

## Back and forth  

DDD is a holistic process and strategic/tactical design should be approached in an iterative manner. If we have doubts while tackling tactical design, there is a sign that we need to reach out to Domain Experts to clarify them.

# Sub-Domains 

While exploring a domain we realize that there are parts that have their specific concerns and thus they should have unique responsibility within their own boundary.

By identifying sub-domains in the strategic phase, we can determine which domains will take us more effort to develop and what should be prioritized.

## 3 Types of Sub-Domains

### Core 
- The most important domain (money maker). [Session reservations domain]
### Supporting 
- Supports the core sub-domain. [Gym Management domain]
### Generic 
- Domain that can either be an in-house development or outsource development work to save time. [Billing Domain]

## Why?

Business/Development time is expensive, so knowing where to prioritize our efforts will be key, especially in the early stages.
## Context mapping

Tool that allows us to visualize the relationship between different parts of our domain.

Meanings within boundaries might differ one from the other even if we talk about the same terms/actors.

In [User Management Context] a user is related to different profiles that we can use (trainers, admins, participants) whereas in [Billing Context] we talk about the billing/payment information of a user.

# Context Map Patterns

- Conformist (CF) 
- Open Host Service (OHS) 
## Team Relationships

- ### Mutually Dependent 
Shared artifacts (need to coordinate).

- ### Free 
Independent (little to no effect on other bounded context).

- ### Upstream – Downstream 
Whatever changes upstream do affects downstream (like the first example provided).

  
## Patterns

- ### Open-Host Service
Provides domain models (like in Context Map patterns [example](https://docs.google.com/document/d/13Hp56HowKhDhlmcmGs2tdjMGXOCIsPGW5K-u_VtYwkI/edit#heading=h.j1pf7jryyhbe)).

- ### Conformist
It conforms with the domain model provided by Bounded Context A (like in Context Map patterns [example](https://docs.google.com/document/d/13Hp56HowKhDhlmcmGs2tdjMGXOCIsPGW5K-u_VtYwkI/edit#heading=h.j1pf7jryyhbe)).

- ### Anti-Corruption Layer 
Translator or adapter that converts data and requests between two bounded contexts, ensuring that Bounded context A does not have to deal with Bounded context B internal model or logic directly.

- ### Shared Kernel 
Shared group of domain models between 2 bounded contexts.

- ### Partnership  
Mutually dependent bounded contexts.

- ### Customer-Supplier 
One-sided dependance as like in [[DDD (Domain Driven Design)#Upstream – Downstream|upstream - downstream]] team relationships.

- ### Published Language 
    

Specification (publicly known), whoever implements it can work in coordination.

- ### Separate Ways 
2 bounded contexts that have nothing to do with one another as like in [[DDD (Domain Driven Design)#Free|free]] team relationships. 

- ### Big Ball of Mud
    

Bounded context that we know is a mess (legacy project), we don’t really want to fix it.

# Problem Space vs Solution Space 

## Problem Space

- This is the area where we understand and define the specific challenge that the software needs to solve, considering all the important details and factors related to that challenge.
## Solution Space 

- This is where we design and build the actual software solution to address the problem identified in the problem space. It encompasses all the technical decisions, architectures, and implementations to create a system that solves the defined challenge. 
  

# Sub-Domains vs Bounded Contexts

As we break down our domain into smaller parts (sub-domains) and assign specific responsibilities to each part, we use bounded contexts to decide how we'll handle those responsibilities and concerns.

  

# Integration Events

Any event that is interesting from a business point of view that crosses the boundary of a Bounded Context.

- We talk about Domain Events when the eventual consistency happens within a boundary, otherwise (across Bounded Contexts) we refer to Integration Events.

## Use Cases

- We propagate those integration events through Message Brokers.
- There must be a resilient error handling logic in place (in case there are issues within bounded contexts and the requests cannot be finished properly).
  
## Outbox pattern

A table in our DB (within the respective Bounded Context) where we put messages at the same time we save changes (transactional consistency) and then in an asynchronous manner we read from there and publish the event(s).