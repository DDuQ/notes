# Definition
Essentially a store for records of data which contains tables; a set of columns which describe the holding values and the rows, the actual value for each record.

# Best practices

## One database per service
Accessing one database per service is the best approach to avoid headaches related to coordination between teams when trying to apply changes on schemas, doing migrations or in general altering the existing template of how data is being stored.

A service needing to interact with another service **must** go through the API, which gives each service freedom to alter its domain as long as the contracts are consistent.

>[!NOTE]
>Allowing a service to be the owner of its database helps to loosely couple our services. 

## Offloading data to an Analytics warehouse
In the case of requiring extensive queries which might hurt the overall database performance for insertions to a database, a good option would be to leverage a standalone warehouse in which those queries would be applied; removing the performance issues.

Leveraging a background worker which would copy the data from the primary database to the analytics warehouse during low traffic hours, ensuring minimal disruption to real-time processes.

## Use of replicas for read paths
While a replica may hurt a primary database momentarily this greatly benefits when taking all the traffic for retrieval of data, naturally balancing the weight of inserts to the primary.

>[!IMPORTANT]
>Diagrams should discriminate the paths for each database so we can easily distinguish primary from replicas.
