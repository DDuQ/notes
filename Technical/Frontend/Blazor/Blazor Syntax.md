When using Blazor, we will see .razor files. Those are the places where pages are defined.
## Directives
### ``@page``
Sets the route for the current page we will be working on. This allows us to reach the page by an identifier.

```csharp
@page "/CodeDemo"  
  
<h3>CodeDemo</h3>  
```

### ``@code``
Block that allows for the creation of variables or logic in general that can be used by the html structure of our page.

```csharp
@*Implicit razor expressions*@  
@name  
  
@*Explicit razor expressions*@  
<p>Current time and date: @(DateTime.Now)</p>

@code {   
string name = "Capy experiment";  
}
```
#### `[Parameter]`
Marks a public property so it can receive a value from a *parent component* or from a *route parameter* (if the component has an `@page` directive with matching placeholders).

**Parent component:** 
```csharp
<!-- Parent.razor -->
<Child Title="Hello Blazor!" />

<!-- Child.razor -->
<h3>@Title</h3>

@code {
    [Parameter]
    public string Title { get; set; } = string.Empty;
}

```

**URL Route:**
```csharp
@page "/pagedemo/{Variable}"

<strong>String Value: </strong> @(Variable??"No string set as param.")  

@code{  
    [Parameter]  
    public string? Variable { get; set; 
}
```

### `@ (Code block)`
Here we have a Blazor code block, which executes immediately when the page is rendered: 

```csharp
@{  
    void RenderName(string name)  
    {  
        <p>Name: <strong>@name</strong></p>  
    }  
      
    RenderName("Capy Bara");  
}
```

**NOTE**: This behaves different than [[Blazor Syntax#``@code``|@code]] directive, since it does not hold state, renders locally and is not aware of the lifecycle.

**When to use then?**
	- Use **`@{ }`** for _lightweight logic_ in markup (variables, inline conditions).
	- Use **`@code { }`** for _real component logic_ like state, event handlers, and lifecycle methods.