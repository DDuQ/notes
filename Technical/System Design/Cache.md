# Definition
A cache serves the purpose of saving a temporal copy for **hot data**, which heavily reduces the amount of requests that hit our databases.

>[!Warning]
>Caches can be shared between services to ensure we don't have duplicated copies of the same hot data, but this comes at a cost of being a critical component.