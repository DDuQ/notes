# Definition

Stands for **S**tructured **Q**uery **L**anguage; a programming language used to manage and manipulate relational databases.

>[!NOTE]
Allows us to ==define schemas, insert, update, delete and query data== in a structured way ensuring the **integrity** and **consistency** of data.

# Order of execution

Even though SQL syntax is written in certain order starting from selected columns, database engines **DO** **NOT** follow the same structure while ==executing queries==.

Example:
`query syntax`
```sql
SELECT item
FROM coats
WHERE color = 'green'
LIMIT 5;
```

`query execution`
```sql
FROM coats -- Checks which will be the source of the data.
WHERE color = 'green' -- Applies the filter.
SELECT item -- Takes the required columns.
LIMIT 5; -- Retrieve rows according to the limit.
```

# Best practices

In professional environments where collaboration with peers is key, there are a set of standard best practices to be followed to enforce the readability of our queries.

Those best practices can be found in [SQL style guide by Simon Holywell](https://www.sqlstyle.guide/).

# Filtering

## WHERE 
This clause allows us to set conditions which will be take into account while processing a query.

For this we'll need to get familiar with comparison operators:

- **`>`** Greater than or after
- **`<`** Less than of before
- **`=`** Equal to
- **`>=`** Greater than or equal to
- **`<=`** less than or equal to
- **`<>`** Not equal to

Regarding strings, we'll surround words with single quotes for comparison (`''`).

## Multiple criteria

To enhance our filtering on queries to approach multiple requirements, there are a set of operators which allows to cover multiple conditions:

|Operator|Description|Example|
|---|---|---|
|**`AND`**|Requires **all** conditions to be true (restrictive filter).|`WHERE Age > 18 AND Country = 'USA'`|
|**`OR`**|Requires **at least one** condition to be true (broad filter).|`WHERE Country = 'USA' OR Country = 'Canada'`|
|**`BETWEEN`**|Filters values **within a range**, often used with numbers or dates.|`WHERE Date BETWEEN '2025-01-01' AND '2025-12-31'`

## Text filters

These operators help you search for text patterns or match specific values:

| Operator       | Description                                                                       | Example                                                         |
| -------------- | --------------------------------------------------------------------------------- | --------------------------------------------------------------- |
| **`LIKE`**     | Finds records **matching a pattern** in a field.                                  | `WHERE Name LIKE 'A%'` → names starting with **A**              |
| **`NOT LIKE`** | Finds records **not matching** a pattern.                                         | `WHERE Name NOT LIKE 'A%'` → excludes names starting with **A** |
| **`IN`**       | Checks if a field matches **any value** in a list (shorthand for multiple `OR`s). | `WHERE Country IN ('USA', 'Canada', 'Mexico')`                  |
  
Those filters can leverage wildcards to be more efficient:

| Symbol  | Description                               | Example                 | Matches                               |
| ------- | ----------------------------------------- | ----------------------- | ------------------------------------- |
| **`%`** | Matches **zero, one, or many characters** | `WHERE Name LIKE 'A%'`  | “Anna”, “Alex”, “Albert”              |
| **`_`** | Matches **exactly one character**         | `WHERE Code LIKE 'A_3'` | “AB3”, “AC3”, but not “A33” or “A123” |

**Position matters:**

- `'A%'` → starts with A
- `'%A'` → ends with A
- `'%A%'` → contains A anywhere
- `'A_%'` → starts with A and has at least one more character


## Missing values

| Operator                | Description                                                                          |
| ----------------------- | ------------------------------------------------------------------------------------ |
| **`COUNT(field_name)`** | Counts **only non-NULL (non-missing)** values in that specific field.                |
| **`COUNT(*)`**          | Counts **all rows** in the result set, **including those with NULLs** in any column. |

## Null values

As well as in [[SQL#Missing values|missing values]], by design ==there **WILL** be null values at some point== due to human error, unknown data or simply information not available yet.

| Operator          | Description                                                   | Example                                                        |
| ----------------- | ------------------------------------------------------------- | -------------------------------------------------------------- |
| **`IS NULL`**     | Returns records where the field **has no value** (`NULL`).    | `WHERE Email IS NULL` → finds rows with no email address.      |
| **`IS NOT NULL`** | Returns records where the field **has a value** (not `NULL`). | `WHERE Email IS NOT NULL` → finds rows with an existing email. |

>[!NOTE]
>It is important to know how to deal with **NULLs** so that we can provide accurate data analysis around them.
>
>Also, take in mind that `NULL` values are [[SQL#Missing values|missing values]].

# Summarizing data

## Aggregate functions

| **Category**           | **Aggregate Function** | **Description**                                                                                                                                                                 |
| ---------------------- | :--------------------: | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Non-numerical data** |         `MIN`          | Returns the smallest (or earliest) value in a set.                                                                                                                              |
|                        |         `MAX`          | Returns the largest (or latest) value in a set.                                                                                                                                 |
|                        |        `COUNT`         | Returns the number of rows (or non-null values).                                                                                                                                |
| **Numerical data**     |         `AVG`          | Returns the average (mean) value of a numeric column.                                                                                                                           |
|                        |         `SUM`          | Returns the total sum of a numeric column.                                                                                                                                      |
|                        |    <br><br>`ROUND`     | Rounds a numeric value to a specified number of decimal places.  <br>Example: <br>`ROUND(price, 2)` → rounds to 2 decimals.  <br>`ROUND(price, -1)` → rounds to the nearest 10. |

>[!IMPORTANT]
>Those aggregates can be mixed with [[SQL#WHERE|WHERE]] statements, so that it can apply said functions to specific subsets of data.

