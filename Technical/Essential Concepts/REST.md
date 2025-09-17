# Definition

**RE**presentational **S**tate **T**ransfer (REST)  is an architecture style for building distributed hypermedia systems.

In order for an API to be RESTful, it will need to follow its constraints.

# Constraints

## Client-server
Both client and server applications MUST be able to evolve separately without any dependency on each other. As long as the contracts are agreed on.
## Uniform interface

Clear defined interface between client and server, that helps *simplifying* and *decoupling* the architecture allowing them to ==evolve independently==.
  
Core principles:  
### **Representation-Based**
interactions are done through a representation (usually JSON or XML).  
### **Resource-Based** 
Everything is treated as a resource, identified by a URI (Uniform Resource Identifier).  
### **Self-Descriptive Messages** 
Each resource representation should contain all the information needed to be processed.  
### **HATEOAS** 
(Hypermedia As The Engine Of Application State) -> The server provides links in responses that tell the client what it can do next.

![[REST-1.jpeg|307x226]]
## Stateless
No client context should be stored on the server between requests (all information should be included). The client is responsible for managing the state of the application.

## Cacheable
Caching should be applied to resources when applicable, and then these resources MUST declare themselves cacheable. Caching can be implemented on the server or client side to improve performance.

## Layered system
A client should not know if it’s connected directly to an end server or some load balancer.


# Idempotency

No matter how many requests are sent to the server, the response should always be the same.

>[!NOTE]
**POST**  is *NOT*  idempotent given that it will change after the first request. 
