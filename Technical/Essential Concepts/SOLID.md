# Definition

**Set of principles** that allows us to create **software designs** that are ==easier to understand, maintain and extend== leading to high quality software that is robust and adaptable.
# Single Responsibility

A class should only have one reason to change; should have only one responsibility or purpose.
# Open / Closed

Software entities (classes, functions, modules, etc.) should be open for extension but closed for modification.
# Liskov Substitution

Objects of a base class should be replaceable with object of its derived class without affecting the correctness of the program.
# Interface Segregation

Clients should not be forced to depend on interfaces they do not use.
# Dependency Inversion

High-level modules should not depend on low-level modules. Both should depend on abstractions.

>[!Important]
>**High-level:** Orchestrator 
>**Low-level:** Implementation

`High-level component`
```csharp
public class OrderService
{
    private readonly IPaymentProcessor _paymentProcessor;

    public OrderService(IPaymentProcessor paymentProcessor)
    {
        _paymentProcessor = paymentProcessor;
    }

    public void PlaceOrder(Order order)
    {
        // Business rule
        _paymentProcessor.Process(order.TotalAmount);
    }
}
```

`Low-level component`
```csharp
public class StripePaymentProcessor : IPaymentProcessor
{
    public void Process(decimal amount)
    {
        // Stripe-specific API call
        Console.WriteLine($"Processing {amount} with Stripe.");
    }
}
```
# Source of concepts

[Master Design Patterns & SOLID Principles in C# - Full OOP Course for Beginners](https://www.youtube.com/watch?v=rylaiB2uH2A&t)
