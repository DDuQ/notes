
There are different approaches that tackle inefficient plan executions for a [[SQL]] query.

Having in mind some of the topics addressed below will help to improve performance given different scenarios.

# Using indexes on joins

Leveraging indexes will help in the initial creation on the SQL statement to early efficiently on rows.

![[Optimize SQL Queries-1.png]]

>[!INFO]
>Both [[B-Tree]] and [[Bitmap Index]] are used for this matter.
>
>Every [[SQL Server#Clustered|clustered]] and [[SQL Server#Non-Clustered|non-clustered]] indexes are stored as [[B-Tree]] structures.

# SARGABLE Queries

Stands for **S**earch **ARG**ument **ABLE**.

This is a query (or condition) where the database can leverage an ==index== to speed up the 
time to retrieve result sets.

>[!INFO]
>SARGABLE queries allow the optimizer to perform [[SQL Server#Index Seek|index seeks]] instead of [[SQL Server#Index Scan|index scans]].

| <center>**SARGABLE**</center>                                                                                  | <center>**NON-SARGABLE**</center>                                                                      |
| :------------------------------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------------------- |
| SELECT * <br>FROM Orders<br>WHERE <br>      OrderDate >= '2025-01-01' <br>	  AND OrderDate < '2026-01-01';<br> | <br>SELECT * <br>FROM Orders<br>WHERE YEAR(OrderDate) = 2025;<br>                                      |
| - The condition compares the **column directly** to constants.                                                 | - Here the **function (`YEAR()`)** is applied the column.                                              |
| - SQL Server can **seek** into the B-tree using `OrderDate` to find the matching date range.                   | - SQL Server must evaluate `YEAR(OrderDate)` for **every row**, so it can’t use the index efficiently. |
| - The predicate is the **search argument** the index can use.                                                  | - The optimizer falls back to an **Index Scan** or even a **Table Scan**.                              |

