# Definition

This is a ==NoSQL document-based ==database.

It's data is stored in [[DynamoDB#Table|tables]], [[DynamoDB#Item|items]], and [[DynamoDB#Attributes|attributes]] rather than traditional databases with rows and columns. 

>[!INFO] Perks
> - Fully managed (no server maintenance required).
> - High performance at any scale.
> - *Data encrypted* at rest; everything is encrypted as is stored.
> - Supports on-demand ==backups and point-in-time recovery==.

Also worth mentioning. It's scalability is defined by a [[DynamoDB#Capacity|credit-based system]].

In there we could adapt both reads and writes per second according to our requirements, which can be scaled automatically by using an AWS Network.

## Table
This is the container for DynamoDB's data. It does **NOT** require a predefined schema.

![[Excalidraw/DynamoDB.excalidraw.md#^clippedframe=OJlPt19lwvEbVhYxwn6R_|200]]
## Item
Single record stored in a table. It gets identified by a primary key ([[DynamoDB#Partition Key|partition key]] or [[DynamoDB#Partition Key + Sort Key|partition + sort key]]).

![[Excalidraw/DynamoDB.excalidraw.md#^frame=YxdbDSjcN2ojVsNXJD0xv|200]]

## Attributes
Describes the data of an item as a key-value pair.

```json
Table: Users
│
├── Item (UserId: 123)
│     ├── Attribute: Name = "Esteban"
│     ├── Attribute: Age = 25
│     └── Attribute: City = "Medellín"
```

# Primary key

There are two forms of a primary key which identifies an item, those are: [[DynamoDB#Partition Key|Partition Key]] and [[DynamoDB#Partition Key + Sort Key|Partition Key + Sort Key]].

## Partition Key
When used, this will be the way to identify uniqueness (as well as the physical storage location) across the whole table.

DynamoDB uses this key to determine which **partition** (internal storage node) the item goes to.

>[!IMPORTANT]
>Every item **must** have a unique partition key value (if there’s no sort key).

## Sort Key
This is a secondary part of a *composite primary key* (together with the [[DynamoDB#Partition Key|partition key]]), allowing multiple items with the **same** partition key to exist, sorted by the sort value.

>[!INFO] Purpose
>Enables the grouping and ordering of related data under the same partition.

## Partition Key + Sort Key
The combination of both [[DynamoDB#Partition Key|partition key]] and [[DynamoDB#Sort Key|sort key]] will define uniqueness across the table.

>[!NOTE]
>[[DynamoDB#Item|Items]] with the same partition key are stored together.


# Capacity

This refers to the **credit-based system** to run DynamoDB, which consists primarily of two parts: **Read Capacity Unit** (RCU) and **Write Capacity Unit** (WCU)
## Read Capacity Unit
Used for retrieving data (querying or scanning tables).

>[!IMPORTANT]
DynamoDB supports three types of reads, each suited for different consistency and accuracy needs:
>- **Eventually Consistent** – Fastest and least expensive; data might be slightly outdated.
>- **Strongly Consistent** – Ensures the most recent data is returned.
>- **Transactional** – Guarantees all-or-nothing consistency when retrieving data.

## Write Capacity Unit
Used to modify data (create, update or delete items).

>[!IMPORTANT]
DynamoDB supports two types of writes:
> - **Standard** – Normal operations for most use cases.
> - **Transactional** – Ensures atomic, consistent, isolated, and durable (ACID) operations across multiple items.

# Conditional Operations

This is a [[DynamoDB#Write Capacity Unit|write]] request which ==will only execute if the specified condition is true==.

If the condition is **NOT MET**, DynamoDB cancels the operation and returns a `ConditionalCheckFailedException`.

## Use cases

- Avoid duplicates:
```csharp
//Only insert an item if it doesn’t already exist.
ConditionExpression = "attribute_not_exists(userId)"
  ```

- Optimistic locking / concurrency control:
```csharp
//Only update if a version number hasn’t changed.
ConditionExpression = "Version = :currentVersion"
//Only update if timestamp hasn’t changed.
ConditionExpression = "UpdatedAt = :currentTime"
```

- Business rule enforcement:
```csharp
//Only delete an order if its status is "Pending".
ConditionExpression = "Status = :order.Status"
```

# Transactions

This is a [[DynamoDB#Write Capacity Unit|transactional write]]. It will allow to make complex operations across different tables and execute them **atomically** — meaning **all succeed or none do**.

>[!WARNING] Limitations
>- Max **100 actions per transaction**.
>- Each item can be up to **4 MB**.
>- All operations count toward your [[DynamoDB#Write Capacity Unit|WCU]].
>- Transactions are **slower** and **more expensive** than normal writes

 Source here -> [TransactWriteItems]([TransactWriteItems - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_TransactWriteItems.html?))

# The scan issue

There is a known issue regarding the retrieve data by filters outside the stablished [[DynamoDB#Primary key|primary keys]]. 

A naive approach would be to scan a whole table and call it a day. Even though it solves the issue it **will** result in a costly operation within DynamoDB since will require to look into every node (created per [[DynamoDB#Partition Key|partition key]]) to match whatever condition.

For this reason, we should always ensure optimal use of the database. One way of addressing this issues is through [[DynamoDB#Secondary indexes|secondary indexes]].
## Secondary indexes
Those are ways of handling different access patterns for our data, though it comes with a cost.

### Global Secondary Index
We can generate an index on a column that is not the [[DynamoDB#Primary key|primary key]]. It allows queries across **all partitions** of the base table — hence the term _global_.

>[!NOTE]
> - Has its own [[DynamoDB#Write Capacity Unit|WCUs]] and [[DynamoDB#Read Capacity Unit|RCUs]].
> - Can be added anytime after a table is created.
 
>[!WARNING]
>==**WILL** duplicate the table and hide it while keeping it in sync with the base table== to provide efficiency when querying the index. 
>- Additional costs on the index maintenance and duplication of data.
>- Duplication of [[DynamoDB#Capacity|capacity]] costs.

### Local Secondary Index
This is a more light-weight option to [[DynamoDB#Global Secondary Index|global secondary indexes]], since it does not need to duplicate data.

Tries to address the same problem of access patterns (scan in general *BAD* / querying *GOOD*).

>[!WARNING] Rules
>- Can **ONLY** be created if there is a composite key ([[DynamoDB#Partition Key + Sort Key|partition key + sort key]]) present in a table.
>- Can **ONLY** be created upon table creation.
>- **CANNOT** be deleted -> Will live for the lifetime of the table.

>[!NOTE] Notes
>- This index leverages the table's partition key -- So we don't worry about separated [[DynamoDB#Write Capacity Unit|WCUs]].
>- [[DynamoDB#Read Capacity Unit|RCUs]] are shared with the base table, so this works like a ternary sort key.


# Streams

This allows to capture and record [[DynamoDB#Item|item-level]] changes (inserts, updates and deletes) in real time, allowing to ==**view** and react to data modifications as they occur in the [[DynamoDB#Table|tables]]==.

There are view types for different scenarios:

*Key attributes only* -> Only [[DynamoDB#Primary key|key attributes]] of the item changed.

*New image* -> New version of a item after changed. 

*Old image* -> Old version of a item before changed.

*New and old images* -> Captures the before and after of the item changed.


# Auto-scaling

