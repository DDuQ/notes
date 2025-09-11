
# Session / Local Storage

Given that data gets wiped once a page is reload, there are a couple of options to give a sense of 'persistence' in our applications. Those options are:

- Blazored session storage 
- Blazored local storage

This might solve that issue but not completely in cases where users wipe the cache or change computers.

# Database

It is possible to use a database directly so we avoid the indirection of requiring an API to provide a resource (and add delay to a response).

The only drawback is that when querying to a database through an ORM such as EntityFramework, we can only query once at a time. 

In this kind of cases we can use DB Context factories.

`This extends from ServiceCollectionExtensions`
```csharp
//NOTE: Any database provider can be used.
services.AddPooledDbContextFactory<TodoDataContext>(options =>
options.UseSqlite("Data Source=TodoApp.db"));

services.AddDbContext<TodoDataContext>(options => options.UseMigrations());

services.AddScoped<ITodoService, TodoServiceDirecAccess>();
```


# Injected Service

As explained in [[Blazor Syntax#Dependency Injection|dependency injection]] we have to be aware on how we want to give a sense of maintained state through the workflows of the app, leveraging those could end in a good interactive experience for users.

NOTE: It is key to use injected services appropriately so behavior is expected:

`Singleton`: State persisted as long as the app is lived.
`Scoped`: State will be persisted until the page is refreshed.

This is useful when we want to short-live state, otherwise we should favor persistence through [[State Management#Database|databases]] instead.

