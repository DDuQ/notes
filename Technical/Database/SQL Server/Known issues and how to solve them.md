
# Login failed for user 'SA'.

While this could potentially happen when the credentials are not properly set, there is a hidden issue when that's not the case.

After hours of debugging, finally found the answer on [StackOverflow]([How to have docker compose init a SQL Server database - Stack Overflow](https://stackoverflow.com/questions/69941444/how-to-have-docker-compose-init-a-sql-server-database)):

> [!IMPORTANT] Solution
> - If your host machine already has a local SQL Server instance running (e.g., via SSMS), stop those services to prevent port conflicts with the Docker container.

For some reason it's like the default port (1433) gets overridden by SSMS, causing the issue -- Making dev life insufferable for awhile. 

Thankfully after stopping SQL Server (MSSQLSERVER) the error just vanished.

![[Known issues and how to solve them-1.png]]

>[!NOTE]
>Ensure to set **Start Mode** as Manual -- Just to make sure we don't incur into the same issue again when working with containers.

# Port issues

## Local environment vs Container

When working with containers it makes sense to have one `appsettings.json` just for the container, so that it can resolve routes based on docker (automatic DNS resolution).

This will avoid finding out where our local env is pointing and having those configs properly set.

For example: 
`Appsettings.json(local env)`
```json
"SQLServerDbSettings": {  
  "ConnectionString": "Data Source=localhost,1433;Initial Catalog=CapybaraPetApp;User Id=sa;Password=Admin123.;TrustServerCertificate=True;Encrypt=False;MultipleActiveResultSets=True;"  
}
```

`Appsettings.json(Development) - set for Docker`
```json
"SQLServerDbSettings": {  
  "ConnectionString": "Data Source=database;Initial Catalog=CapybaraPetApp;User Id=sa;Password=Admin123.;TrustServerCertificate=True;MultipleActiveResultSets=True;"  
}
```

Here '**database**' will be resolved by Docker internally -- pointing to the correct database port.