# Definition

UI framework built in HTML, CSS and C#. Enables the creation of interactive web application running C#. Allowing to leverage .NET backend background to reach a full stack environment.

It can pre-render content in the server and hand over to [[WebAssembly]] for client-side operations. Also supports scenarios where no client-side execution is needed at all. 

Supports static pages, server run pages, client run pages as well as desktop and mobile through .NET MAUI.

# What makes it great

- Simple and familiar way that Blazor gives us as an interactive UI. 
- It can leverage [[SignalR]] in order to update UI as events happens.
- Easy to make reusable components.

# Blazor Server

Runs on the server using [[SignalR]], maintaining a dynamic and responsive user interface.

Advantages:
- Minimal code needs to be sent to the client.
- Optimized load times and efficiency (quick startup times).
- Useful for rapid interaction feedback.

Disadvantages:
- Requires constant connectivity (no support for offline mode).
- Latency issues due to round-trips to the databases per user.
- Server load increases with each connection (complicating scaling).
- Increased memory usage.

## How it works

![[Blazor-1.jpeg]]
We have a server that contains Razor components, those are rendered into Html.
Blazor creates a render tree; a digital representation of the DOM.

To interact with the web, [[SignalR]] serves as a bridge to communicate changes from the server and to render only what has changed in the UI.

# Blazor WebAssembly

Runs directly on the client-side. 

Advantages:
- Mainly useful for webpages that run offline.
- Ideal for PWAs or applications that need to function in low connectivity environments.
- Not server dependent.
- Enhanced UI responsiveness.
- Faster screen refreshes.

Disadvantages:
- The initial load time may be longer.
- Large footprint (many files to download).
- Heavy dependence on Web APIs.

**NOTE**: JSInterop is used in order to modify the DOM.
## How it works

Normally JavaScript would take the route:
JavaScript > Parse > Compiler > JIT Interpreter

Whilst [[WebAssembly]] can bypass this, since the interpreter can run it.

WebAssembly > JIT Interpreter

# Should I use Blazor Server or WebAssembly?

Normally it depends on the type of website to build. 

There is an alternative as well to leverage both options; Auto mode, which essentially runs on SignalR in the first load to then use WebAssembly later on.