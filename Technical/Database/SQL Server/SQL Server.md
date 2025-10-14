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
>**Optimization** is the process of transforming a query’s logical plan (_what_ to retrieve) into an efficient physical plan (_how_ to retrieve it), by choosing the best combination of algorithms and access paths that the execution engine can perform.
>
>Logical:  Scan --> Filter --> Project
  Physical: Index Seek --> Filter --> Output

### Execution plan
Runs the best physical plan chosen during the [[SQL Server#Optimization process|optimization process]] to produce the query’s result set.

>[!INFO]
>Result set refers to the data retrieved by the SQL Query after its execution.
>
>In other words, it’s the _set of rows and columns_ that match the conditions of said query.


# Clustered / Non-Clustered Indexes

## Clustered
Determines the physical order of rows in a table. --> **direct access**.

>[!IMPORTANT]
>Data is stored on disk in the same order as the index key — so the leaf level of the index _is_ the actual data.
>
>**Only one** clustered index can be created per table, often created on the table's primary key.

## Non-Clustered
Defines a separate structure that stores the indexed columns plus a pointer to the actual data row. --> **indirect access via a pointer**.

>[!IMPORTANT]
> - Multiple non-clustered indexes can be created per table.
> - Data **IS NOT** physically sorted by this index.


# Seeks vs Scans

## Index Seek
This means that the [[SQL Server#Engine|engine]] was able to use an **index** efficiently; jumping straight to the relevant rows.

Like this:
```sql
SELECT * FROM Users WHERE Id = 5;
```

Here the user will be easily retrieved given that is directly accessed through its index.
## Index Scan
This means that the [[SQL Server#Engine|engine]] had to read the entire index (or big portion of it), checking for matches.

For instance:
```sql
SELECT * FROM Days WHERE Name LIKE '%day%';
```

Here assuming `Name` column has an index, it wouldn't be able to use it given the wildcard `%` at the beginning and end of the word.

We will be looking for all matches that include 'day'; requiring to scan the whole index (on `Name` column).

## Table Scan
When there is not indexes at all for a given condition, the [[SQL Server#Engine|engine]] will scan the entire table, row by row.

```sql
SELECT * FROM Users WHERE Age = 30;
```

If `Age` isn’t indexed --> the [[SQL Server#Engine|engine]] has to read all rows to find matches.