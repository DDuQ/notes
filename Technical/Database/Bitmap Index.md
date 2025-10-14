# Definition

This is an special type of database index that uses bitmaps (**bit arrays**) ==instead of tree structures== to represent specific column values that exist in a table.

![[Bitmap Index-1.png]]

>[!INFO]
>  - Bitmap index is specially fast to handle AND/OR conditions; given the use of bitwise operations.
>  - Those indexes may be internally used as ==temporary filters== by the [[SQL Server#Optimization process|query optimizer]] during joins or scans.


