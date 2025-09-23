# Definition

This is essentially a pointer to a function. Meaning that we can receive functions as params and use them as variables.

```csharp
public delegate bool NumberFilter;

NumberFilter isEvenFilter = n => n % 2 == 0;

ProcessNumbers([1,2,3,4,5], isEvenFilter);

static void ProcessNumbers(int[] numbers, NumberFilter filter)
{
	foreach (number in numbers)
	{
		if(filter(number))
		{
			Console.WriteLine($"{number} added after filtering.");
		}
	}
}
```

# Types

## Func
Used for methods that return a value.

>[!IMPORTANT]
>The last type parameter is the type returned.
>
>Also, this type should be favored as **Func<T, bool>** instead of [[Delegates#Predicate|predicates]].

```csharp
Func<int, int, int> Add = (x, y) => x + y;

Console.WriteLine(Add(2,5));
```

## Action
Used for methods that don't return anything (void).

```csharp
Action<string> LogSomething = (something) => Console.WriteLine(something);

LogSomething("Something I wanna log.");
```

## Predicate
Used for methods that return *bool*.

```csharp
Predicate<int> isEven = x => x % 2 == 0;

Console.WriteLine(isEven(5));
```

>[!INFO]
>This was introduced into C# before [[Delegates#Func|Func]].

