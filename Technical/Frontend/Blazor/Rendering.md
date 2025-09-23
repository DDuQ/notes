# Lifecycle events

There are a set of lifecycle methods that are part of the component base class. We can override them trigger at the right time.

>[!NOTE] Note
It is **recommended** to use render modes per the whole application instead of per component, given that we could lose some of the lifecycle functionality.

## `SetParametersAsync` 
Takes in a *ParameterView* (all the params from parent component).

```csharp
public override Task SetParametersAsync(ParameterView parameters)
{
	LogEvent("SetParametersAsync");
	return base.SetParametersAsync();
}
```

## `OnInitialized` & `OnInitializedAsync`
This runs when the component is first created. Here we can setup initial state or fetch initial data.

>[!NOTE]
 It will run once when the component is rendered. 
 When running per component mode it behaves differently.

>[!WARNING]
`OnInitializedAsync` should be preferred when we are using asynchronous methods.

```csharp
protected async override void OnInitialized()
{
	LogEvent("OnInitialized");
}

protected async override Task OnInitializedAsync()
{
	LogEvent("OnInitializedAsync");
	return base.OnInitializedAsync();
}
```

## `OnParametersSet` & `OnParametersSetAsync`
This triggers when the initial setup is done and whenever components' parameters change. 

Useful when we want to react to changes in the data passed to a component (e.g. get data from a database that depends on parameters)

```csharp
protected override void OnParametersSet()
{
    LogEvent("OnParametersSet");
    base.OnParametersSet();
}

protected override Task OnParametersSetAsync()
{
    LogEvent("OnParametersSetAsync");
    return base.OnParametersSetAsync();
}
```

## `OnAfterRender` & `OnAfterRenderAsync`
Triggers after components render.

This is the right place we want to add interactions with components that are already rendered (e.g. trigger JS calls that interact with those rendered components)

 ```csharp
 protected override void OnAfterRender(bool firstRender)
{
    Events.Add($"OnAfterRender is first render:{firstRender}");
}

protected override Task OnAfterRenderAsync(bool firstRender)
{
    Events.Add($"OnAfterRenderAsync is first render:{firstRender}");
    return base.OnAfterRenderAsync(firstRender);
}
 ```

## `ShouldRender`
Controls whether we should re-render components or not.

>[!NOTE]
First render will always happen and we can control renders from there.

```csharp
protected override void ShouldRender()
{
	Events.Add("ShouldRender");
	return true;
}
```


# Prerendering

Good way of showing content fast and including [[SEO]]. 

This adds complexity like making sure we don't access browser resources before the app actually loads.

## Persistent component state

Allows to **save and restore component state** across different user interactions or page reloads, even if the component is disposed of and re-rendered.

It’s especially useful for:

- **Preserving UI state** across page reloads or navigations.
- **Avoiding unnecessary data fetching** when the state can be restored.
- **Improving user experience** by keeping form values, filters, or preferences intact.

### Demo
```csharp
@page "/weather"
@inject PersistentComponentState PersistentState

<h3>Weather Forecast</h3>

@if (forecasts is null)
{
    <p><em>Loading...</em></p>
}
else
{
    <ul>
        @foreach (var forecast in forecasts)
        {
            <li>@forecast</li>
        }
    </ul>
}

@code {
    private string[]? forecasts;
    private PersistingComponentStateSubscription? subscription;

    protected override void OnInitialized()
    {
        PersistentState.TryTakeFromJson<WeatherForecast[]>("forecasts", out forecasts);
		if (forecasts == null)
		{
		    await Task.Delay(500);
		
		    var startDate = DateOnly.FromDateTime(DateTime.Now);
		    var summaries = new[] { "Freezing", "Bracing", "Chilly", "Cool", "Mild", "Warm", "Balmy", "Hot", "Sweltering", "Scorching" };
		    forecasts = Enumerable.Range(1, 5).Select(index => new WeatherForecast
		    {
		        Date = startDate.AddDays(index),
		        TemperatureC = Random.Shared.Next(-20, 55),
		        Summary = summaries[Random.Shared.Next(summaries.Length)]
		    }).ToArray();
		}
		PersistentState.RegisterOnPersisting(() => 
		{
			if(forecasts != null)
			{
				PersistentState.PersistAsJson("forecasts", forecasts);
			} 
			
			return Task.CompletedTask;
		});
    }
}
```


# Render Modes

`<Routes />` component allows us to set the render mode in which the whole app will run with.

We do this by setting the directive `@rendermode`. This could also be done per component:

```csharp
@page "/Rendermode"
@using BlazorWebAppDemo.Client.Components.Demos.RenderMode
<h3>RenderModeDemo</h3>

<!-- Static (instantaneous but no behavior) -->
<NewCounter />

<!-- Interactive with pre-rendering (faster) -->
<NewCounter @rendermode="InteractiveServer" />
<NewCounter @rendermode="InteractiveWebAssembly" />

<!-- Interactive without pre-rendering (slower) -->
<NewCounter @rendermode="@(new InteractiveServerRenderMode(prerender:false))" />
<NewCounter @rendermode="@(new InteractiveWebAssemblyRenderMode(prerender:false))" />
```


# Server-Side Rendering (SSR)

## Static Server-Side Rendering

Everything is rendered on the server, will display no behavior, just sends data to the client.

## Streaming Server-Side Rendering

Everything is rendered on the server but as it sends data in batches, giving the feeling of "interactivity".



