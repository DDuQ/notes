# Definition

This is an [[ORM]].

>[!IMPORTANT]
It’s **cross-platform**, works with multiple databases (SQL Server, PostgreSQL, MySQL, SQLite, etc.), and is the successor of Entity Framework 6.

# DbContext

This is the base class provided by Entity Framework, it is in charge of opening/closing connections to the database when required.

>[!INFO] Manages
 **Database connections** (opens/closes when needed).
 **Change tracking** (detects changes to entities so it knows what to insert/update/delete).
 **LINQ to SQL translation** (turns LINQ queries into SQL).
 **Unit of Work pattern** (groups operations into a transaction).

Through DbContext, [[Linq]] **queries** (selects) get returned as materialized C# classes and **commands** (inserts, updates, deletes) get executed on the database.

![[Excalidraw/EntityFramework.excalidraw.md#^frame=ExM4P6o4rgSxmTB5qSrB_|700]]

## How it works?

1. Identifies which classes in our code represent data that should be stored in the database.
2. Understands the database schema (tables, columns) where queries and commands will run.
3. Maps classes to tables so that changes in objects are translated into changes in the database.

![[Excalidraw/EntityFramework.excalidraw.md#^frame=PEt8d1IfbEplPJl2borHK|700]]

## OnModelCreating
This a method where we can feed entity framework information about the mapping between the code and the database.

Thus we could override it to use the conventions we would like.

Here we have a `ModelBuilder` (which implements the [[Builder|builder]] pattern), which gives us methods to configure our conventions.

```csharp
public class CapybaraPetAppDbContext : DbContext  
{
	protected override void OnModelCreating(ModelBuilder modelBuilder)  
	{  
	    modelBuilder.
	    ApplyConfigurationsFromAssembly(Assembly.GetExecutingAssembly());  
	    base.OnModelCreating(modelBuilder);  
	}
}
```

```csharp
public class ItemConfiguration : IEntityTypeConfiguration<Item>  
{  
    public void Configure(EntityTypeBuilder<Item> builder)  
    {  
        builder.ToTable(nameof(Item));  
  
        builder.OwnsOne(i => i.ItemDetail);  
    }  
}
```

>[!INFO]
>This is the recommended way of leveraging `OnModelCreating`; by calling all the configurations through the assembly.
>*Applies as long as we are using a single database*



