Parameters are passed as a copy or as an alias.

- Copy passes the value (data or reference).
- in, out and ref are passed as aliases with different mutability rules.

>[!INFO] Mutability rules
>**in** -> Read-only alias.
>**out** -> Must be assigned by the caller.
>**ref** -> Receives ref from caller (aimed for structs mainly) and can mutate. 
>**ref readonly** ->  Receives ref from caller (aimed for structs mainly), *cannot* mutate.
# Mental model

![[Passing Parameters-1.png]]
