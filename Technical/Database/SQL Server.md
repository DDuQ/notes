# Definition

Relational database created by Microsoft (Uses [[SQL]] as the language to interact with the [[SQL Server#Engine|engine]]).

# Engine

## Storage Engine
Responsible for reading data between the disk and memory in such way that optimizes concurrency while maintaining integrity.

![[Excalidraw/SQL Server.excalidraw.md#^frame=fKL5BTuaVR1hY4rrUfEZ5|170]]

## Query processor
Accepts queries provided to SQL Server, using T-SQL.
The first tasks of the query processor is to provide an execution plan:

![[Excalidraw/SQL Server.excalidraw.md#^frame=xUDNxSP7Luwur-64QAh16|200]]

### Steps to process a query

Initially from a SQL Statement there will be 2 parts involved:
#### Parsing
Makes sure that T-SQL has a valid SQL query and translates it into an initial tree representation.
#### Binding (name resolution)
Makes sure all the objects do exist and associates every table and column to the parsed tree with their corresponding object in the *system catalog*.

>[!NOTE]
>**System catalog** is a set of special tables maintained by the database engine that store metadata about all database objects.
>
>Also this process results in an algebraized tree, which is then used by the optimization process.

### Optimization process

Here the algebraized tree from [[SQL Server#Binding (name resolution)|binding]] process will be leveraged:
#### Optimize 
Generates multiple candidates for the execution plan according to their costs.

>[!INFO]
>**Optimization** is the process of transforming a query’s logical plan (which describes _what_ to retrieve) into an efficient physical plan (which defines _how_ to retrieve it), by choosing the best combination of algorithms and access paths that the execution engine can perform.
>
>Logical:  Scan → Filter → Project
↓
Physical: Index Seek → Filter → Output

### Execution plan
Runs the best physical plan chosen during the [[SQL Server#Optimization process|optimization process]] to produce the query’s result set.

>[!INFO]
>Result set refers to the data retrieved by the SQL Query after its execution.
>
>In other words, it’s the _set of rows and columns_ that match the conditions of said query.


