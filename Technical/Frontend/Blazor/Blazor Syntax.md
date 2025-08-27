
# Concept

When using Blazor, we will see .razor files. Those are the places where pages are defined.

# Dependency Injection

As well as in ASP.NET, we have 3 different lifecycles: *Singleton*, *Scoped* and *Transient*.

Those differ a bit different from the backend solutions. So it's better to be clear on how they work:

`Singleton`: Shares one instance for all our users.
	With WebAssembly this means only local instance.

`Scoped`: Creates a new instance per session.
	- **Blazor Server** has no sessions; it will create a new instance per circuit (SignalR connection). Then if the page gets reloaded, SignalR connection will be lost.
	- **WebAssembly** has no sessions either; service will live as long as is loaded.

`transient`: New instance per request.
	This works the same regardless of the hosting model.

# Directives
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

### `@typeparam`
This allows us to use generics for our components.

```csharp
<!-- Use implementation -->
<MyList TItem="string" Items="@data" />

@*The type can be implied since it is generic*@
<MyList Items="@data2" />

@code {
    private List<string> data = new () { "Apple", "Banana", "Cherry" };
    private List<int> data2 = new() { 42, 73, 1337, 1701, 1138 };
}

<!-- Generic implementation to iterate on items -->
@typeparam TItem
@if (Items != null)
{
    foreach (var item in Items)
    {
        <div>@item?.ToString()</div>
    }
}

@code {
    [Parameter]
    public IEnumerable<TItem>? Items { get; set; }
}
```

### `@attribute`
It allows to use any kind of attribute in our components, normally used for authorization.

```csharp
@using Microsoft.AspNetCore.Authorization

@attribute [Authorize]

<h3>AttributeDemo</h3>

@code {
}
```

### `@attributes`
Used to **capture and pass through additional attributes** from the parent component to an HTML element in the child component.

```csharp
<!-- Parent -->
<MyButton class="btn btn-primary" id="saveBtn" />

<!-- MyButton.razor -->
<button @attributes="AdditionalAttributes">Click me</button>

@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object>? AdditionalAttributes { get; set; }
}
```

#### How is this different from [[Blazor Syntax#`@attribute`|@attribute]]

| Directive     | Purpose                                                                           |
| ------------- | --------------------------------------------------------------------------------- |
| `@attribute`  | Adds a **C# attribute** to the generated component class. Example: `[Authorize]`. |
| `@attributes` | Adds **HTML attributes** to a rendered HTML element inside a component.           |

### `@using` and \_Imports.razor 
Using directive allows us to import specific components, functionalities to be available in the current page.

```csharp
@using System.Linq
```

On the other hand we have *_Imports.razor*  file which allows to centralize all those using directives into a single place so that all the files in the current folder and sub-folders will inherit them.

```csharp
<!-- _Imports.razor -->
@using System.Net.Http
@using System.Net.Http.Json
@using Microsoft.AspNetCore.Components.Forms
@using Microsoft.AspNetCore.Components.Routing
@using Microsoft.AspNetCore.Components.Web
@using static Microsoft.AspNetCore.Components.Web.RenderMode
@using Microsoft.AspNetCore.Components.Web.Virtualization
@using Microsoft.JSInterop
@using BlazorWebAppDemo
@using BlazorWebAppDemo.Client
@using BlazorWebAppDemo.Components
```

### `@layout`
This tells a Razor component to use a **layout component** as a wrapper.

- Layout components act like **templates**:  
    They define a common structure (header, sidebar, footer) and use `@Body` as a placeholder for the page content.

- Individual pages/components specify which layout to use via `@layout`.
  
```csharp
@inherits LayoutComponentBase
<div class="page">
    <header>
        <h1>My App</h1>
    </header>
    <main>
        @Body
    </main>
    <footer>
        <p>Footer content</p>
    </footer>
</div>
```
  
**NOTE**: Instead of writing `@layout MainLayout` on every page, it can be set globally in **`_Imports.razor`**:

```csharp
<!-- _Imports.razor -->
@layout MainLayout
```

Now all pages use `MainLayout` unless it's overridden.

### `@onclick` & Event Callbacks
This directive allows us to call a method with custom logic whenever the event gets triggered (on click).

```csharp
<!-- Parent -->
<MyButton class="btn btn-primary" OnClick="IncrementCount">Click me</MyButton>

@code{
	private int currentCount = 0;
	
	private void IncrementCount()
	{
		currentCount++;
	}
}

<!-- MyButton.razor -->
<button @attributes="AdditionalAttributes" @onclick="OnButtonClickedAsync">Click me</button>

@code {
    [Parameter(CaptureUnmatchedValues = true)]
    public Dictionary<string, object>? AdditionalAttributes { get; set; }
    
    [Parameter]
    public EventCallback OnClick { get; set; }
    
    private async Task OnButtonClickedAsync()
    {
	    await OnClick.InvokeAsync();
    }
}
```

When the parent sets `OnClick="IncrementCount"`, it assigns its `IncrementCount` method to the `OnClick` parameter of `MyButton` (an EventCallback).

When the button is clicked, `OnButtonClickedAsync` runs inside the child component and calls `OnClick.InvokeAsync()`, triggering the parent’s `IncrementCount` method.

![[Blazor Syntax-1.jpeg|540x360]]

# Binding

#### `@bind`
Provides two-way data binding between a component property and a UI element’s value.

#### `@bind-value` (Case-sensitive)
Used for **HTML elements** (like `<input>`).
    Explicitly binds to the lowercase HTML attribute `value`.

```csharp
<input @bind-value="username" />
```
Here, `username` syncs with the HTML input’s `value` attribute.

#### `@bind-Value` (Case-sensitive)
Used for **Blazor components** that expose a parameter `Value` with `ValueChanged`.

```csharp
<InputText @bind-Value="username" />
```
This uses the Blazor component’s property `Value` and event `ValueChanged`.

#### `@bind-value:event`
Allows customizing the event that triggers the binding update instead of the default (`onchange` for inputs).

```csharp
<input @bind-value="textValue" @bind-value:event="oninput" />
```

#### `@bind-value:after`
Executes code **after** the binding updates the value.

```csharp
<input @bind-value="textValue" @bind-value:after="OnTextChanged" />

@code {
    private string textValue;
    private void OnTextChanged() => Console.WriteLine(textValue);
}
```

#### `@bind-value:get` & `@bind-value:set`
Gives **full control** over binding by explicitly defining getter and setter logic.

```csharp
<input 
    @bind-value:get="() => textValue" 
    @bind-value:set="val => textValue = val.ToUpper()" />
```

# Custom Components

One key feature of components is how we manage bindings.  To handle binding we will require 3 parameters (according to Blazor convention on [Microsoft docs](https://learn.microsoft.com/en-us/aspnet/core/blazor/components/data-binding?view=aspnetcore-9.0#:~:text=The%20following%20ChildBind,named%20YearChanged.)):

- `Value`: Name of the property.
- `ValueChanged`: Name of the property followed by *Changed*.
- `ValueExpression`(**optional**): Name of the property followed by *Expression*. 

```csharp
@code{
	[Parameter]
	public string Value { get; set; }
	[Parameter]
	public EventCallback<string> ValueChanged { get; set; }
	[Parameter]
	public Expression<Func<string>> ValueExpression { get; set; }
}
```

## Demo

`ParentComponent.razor`
```csharp
@* This wont work, as it binds one way. *@
<BindingComponent Value="@bindingComponentValue1" /> @bindingComponentValue1

@* Here we automatically set two way binding. *@
<BindingComponent @bind-Value="@bindingComponentValue2" /> @bindingComponentValue2

@* Manually sets two way binding with the event handler. *@
<BindingComponent Value="@bindingComponentValue3" ValueChanged="ValueChangedMethod" /> @bindingComponentValue3

@code {
    string bindingComponentValue1 = "Before update";
    string bindingComponentValue2 = "Before update";
    string bindingComponentValue3 = "Before update";
    private void ValueChangedMethod(string value)
    {
	    bindingComponentValue3 = value;
    }
}
```

`BindingComponent.razor` 
```csharp
<button @onclick="UpdateValueAsync">Update me</button>
@code{
	[Parameter]
	public string Value { get; set; }
	[Parameter]
	public EventCallback<string> ValueChanged { get; set; }
	[Parameter]
	public Expression<Func<string>> ValueExpression { get; set; }
	
	private async Task UpdateValueAsync()
	{
		Value = DateTime.Now.ToString();
		<!-- This is required in order to update Value. -->
		await ValueChanged.InvokeAsync(Value); 
	}
}
```
