When an application is vulnerable to SQL injection, and the results of the query are returned within the application's responses, you can use the `UNION` keyword to retrieve data from other tables within the database. This is commonly known as a SQL injection UNION attack.

The `UNION` keyword enables you to execute one or more additional `SELECT` queries and append the results to the original query. For example:

```sql
SELECT a, b FROM table1 UNION SELECT c, d FROM table2
```

This SQL query returns a single result set with two columns, containing values from columns `a` and `b` in `table1` and columns `c` and `d` in `table2`.

> [!info] Requirements for a UNION query
> For a `UNION` query to work, two key requirements must be met:
> 
> - The individual queries must return the same number of columns.
> - The data types in each column must be compatible between the individual queries.
> 
> To carry out a SQL injection UNION attack, make sure that your attack meets these two requirements. This normally involves finding out:
> 
> - How many columns are being returned from the original query.
> - Which columns returned from the original query are of a suitable data type to hold the results from the injected query.

---
## Determining the number of columns required
When you perform a SQL injection UNION attack, there are two effective methods to determine how many columns are being returned from the original query.

1) **ORDER BY with Index Increment**
One method involves injecting a series of `ORDER BY` clauses and incrementing the specified column index until an error occurs. For example, if the injection point is a quoted string within the `WHERE` clause of the original query, you would submit:

```sql
' ORDER BY 1-- 
' ORDER BY 2-- 
' ORDER BY 3-- 
etc.
```

This series of payloads modifies the original query to order the results by different columns in the result set. The column in an `ORDER BY` clause can be specified by its index, so you don't need to know the names of any columns. 

When the specified column index exceeds the number of actual columns in the result set, the database returns an error, such as:

```http
The ORDER BY position number 3 is out of range of the number of items in the select list.
```

The application might actually return the database error in its HTTP response, but it may also issue a generic error response. In other cases, it may simply return no results at all. Either way, as long as you can detect some difference in the response, you can infer how many columns are being returned from the query.

2) **ORDER BY with NULL value(s)**
The second method involves submitting a series of `UNION SELECT` payloads specifying a different number of null values:

```sql
' UNION SELECT NULL-- 
' UNION SELECT NULL,NULL-- 
' UNION SELECT NULL,NULL,NULL-- 
etc.
```

If the number of nulls does not match the number of columns, the database returns an error, such as:

```http
All queries combined using a UNION, INTERSECT or EXCEPT operator must have an equal number of expressions in their target lists.
```

> [!INFO] Why use NULL
> We use `NULL` as the values returned from the injected `SELECT` query because the data types in each column must be compatible between the original and the injected queries. `NULL` is convertible to every common data type, so it maximizes the chance that the payload will succeed when the column count is correct.

> [!attention] NullPointerException
> The effect on the HTTP response depends on the application's code. If you are lucky, you will see some additional content within the response, such as an extra row on an HTML table. Otherwise, the null values might trigger a different error, such as a `NullPointerException`. In the worst case, the response might look the same as a response caused by an incorrect number of nulls. This would make this method ineffective.

---
## Finding columns with useful data type
A SQL injection UNION attack enables you to retrieve the results from an injected query. The interesting data that you want to retrieve is normally in string form. This means you need to find one or more columns in the original query results whose data type is, or is compatible with, string data.

After you determine the number of required columns, you can probe each column to test whether it can hold string data. You can submit a series of `UNION SELECT` payloads that place a string value into each column in turn. 

For example, if the query returns four columns, you would submit:

```sql
' UNION SELECT 'a',NULL,NULL,NULL-- 
' UNION SELECT NULL,'a',NULL,NULL-- 
' UNION SELECT NULL,NULL,'a',NULL-- 
' UNION SELECT NULL,NULL,NULL,'a'--
```

If the column data type is not compatible with string data, the injected query will cause a database error, such as:

```http
Conversion failed when converting the varchar value 'a' to data type int.
```

If an error does not occur, and the application's response contains some additional content including the injected string value, then the relevant column is suitable for retrieving string data.

---
## Using a SQL injection UNION attack to retrieve interesting data
When you have determined the number of columns returned by the original query and found which columns can hold string data, you are in a position to retrieve interesting data.

Suppose that:

- The original query returns two columns, both of which can hold string data.
- The injection point is a quoted string within the `WHERE` clause.
- The database contains a table called `users` with the columns `username` and `password`.

In this example, you can retrieve the contents of the `users` table by submitting the input:

```sql
' UNION SELECT username, password FROM users--
```

In order to perform this attack, you need to know that there is a table called `users` with two columns called `username` and `password`. Without this information, you would have to guess the names of the tables and columns. All modern databases provide ways to examine the database structure, and determine what tables and columns they contain.

---
### Retrieving multiple values within a single column
In some cases the query in the previous example may only return a single column.

You can retrieve multiple values together within this single column by concatenating the values together. You can include a separator to let you distinguish the combined values. 

> [!important] Oracle DOES NOT like spaces!
> Oracle does not like space between the end of the injected query and the comment beginning!

> [!example] Oracle String Concat
> For example, on Oracle you could submit the input:
> 
> ```sql
> ' UNION SELECT username || '~' || password FROM users--
> ```

This uses the double-pipe sequence `||` which is a string concatenation operator on Oracle. The injected query concatenates together the values of the `username` and `password` fields, separated by the `~` character. 
Refer to the Cheatsheets Canvas for other DB string concat versions (MSSQL, MYSQL, PostgreSQL)

---
## Examining the database in SQL injection attacks
To exploit SQL injection vulnerabilities, it's often necessary to find information about the database. This includes:

- The type and version of the database software.
- The tables and columns that the database contains.

You can potentially identify both the database type and version by injecting provider-specific queries to see if one works

The following are some queries to determine the database version for some popular database types:

| Database type    | Query                     |
| ---------------- | ------------------------- |
| Microsoft, MySQL | `SELECT @@version`        |
| Oracle           | `SELECT * FROM v$version` |
| PostgreSQL       | `SELECT version()`        |

> [!important] Use NULL for testing
> Make sure you have tested the columns (using NULL) where the query would return some data

---
## Listing the contents of the database
Most database types (except Oracle) have a set of views called the information schema. This provides information about the database.

For example, you can query `information_schema.tables` to list the tables in the database:

```sql
SELECT * FROM information_schema.tables
```

This returns output like the following:

| TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME | TABLE_TYPE |
| ------------- | ------------ | ---------- | ---------- |
| MyDatabase    | dbo          | Products   | BASE TABLE |
| MyDatabase    | dbo          | Users      | BASE TABLE |
| MyDatabase    | dbo          | Feedback   | BASE TABLE |

This output indicates that there are three tables, called `Products`, `Users`, and `Feedback`.

You can then query `information_schema.columns` to list the columns in individual tables:

```sql
SELECT * FROM information_schema.columns WHERE table_name = 'Users'
```

This returns output like the following:

| TABLE_CATALOG | TABLE_SCHEMA | TABLE_NAME | COLUMN_NAME | DATA_TYPE |
| ------------- | ------------ | ---------- | ----------- | --------- |
| MyDatabase    | dbo          | Users      | UserId      | int       |
| MyDatabase    | dbo          | Users      | Username    | varchar   |
| MyDatabase    | dbo          | Users      | Password    | varchar   |

This output shows the columns in the specified table and the data type of each column.

> [!important] Getting table names and column names from information_schema 
> SQLi to get table names from information schema:
> ```sql
> 'UNION SELECT NULL,table_name FROM informaton_schema.tables --
> ```
> 
> SQLi to get column names from a specific table in information schema:
> ```sql
> 'UNION SELECT NULL,column_name FROM information_schema.columns WHERE table_name='users_zhzrbx' --
> ```
> Note: NULL is used in this example because it suited
> 

#security/web/sql 