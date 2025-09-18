# Definition

Ensures a class has only one instance and provides a global point of access to that instance. 

>[!INFO]
The single instance is commonly used for **managing shared resources**, **configuration settings**, or **logging** functionality within an application.

# Implementation

The singleton class will have an `instance` field and a `getInstance` method.

`getInstance` will create a new `instance` the first time it gets called and return instance for the following calls.

![[Singleton.excalidraw|500]]

## Code Example 

```csharp
public class Logger
{
	private static Logger _instance;
	
	private Logger() { }
	
	public Logger getInstance()
	{
		if(_instance is null)
		{
			_instance = new Logger();
		}
		
		return _instance;
	}
}
```
