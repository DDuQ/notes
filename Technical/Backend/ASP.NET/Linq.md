# Definition

Stands for **Language Integrated Query**, this is a feature in .NET that lets us to write queries directly on C# (or VB.NET).

>[!IMPORTANT]
Allows us to **query** ==collections, databases, XML documents, or any data source== using the same syntax.

# Advantages

- Type-safe (checked at compile-time).
- Intuitive and readable syntax.
- Removes the need to mix SQL queries or other query languages.
- Works consistently across different data sources.

# Deferred execution

This means that [[Linq]] is not executed automatically when defined -- it only runs when the collection is actually materialized or used (e.g. iterated).

`iteration`
```csharp
var numbers = new List<int> { 1, 2, 3, 4, 5 };

// Query is defined but not executed yet
var evens = numbers.Where(n => n % 2 == 0);

// Now we enumerate it (execution happens here)
foreach (var n in evens)
{
    Console.WriteLine(n);
}
```

`materialization`
```csharp
// Defines query but not executes yet
var query = context.Capybaras.Where(c => c.Age > 3);

// Materialized --> Executes
var result = query.ToList();
```

>[!NOTE]
>These are the *materialization methods*:
>- `.ToList()`
>- `.ToArray()`
>- `.Count()`
>- `.FirstOrDefault()`
>- `.Any()`

So, in reality we're building a query object, not executing it yet.