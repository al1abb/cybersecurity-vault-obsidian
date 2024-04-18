When an application is vulnerable to SQL injection, and the results of the query are returned within the application's responses, you can use the `UNION` keyword to retrieve data from other tables within the database. This is commonly known as a SQL injection UNION attack.

The `UNION` keyword enables you to execute one or more additional `SELECT` queries and append the results to the original query. For example:

```sql
SELECT a, b FROM table1 UNION SELECT c, d FROM table2
```

This SQL query returns a single result set with two columns, containing values from columns `a` and `b` in `table1` and columns `c` and `d` in `table2`.

> [!Requirements for a UNION query]
> For a `UNION` query to work, two key requirements must be met:
> 
> - The individual queries must return the same number of columns.
> - The data types in each column must be compatible between the individual queries.
> 
> To carry out a SQL injection UNION attack, make sure that your attack meets these two requirements. This normally involves finding out:
> 
> - How many columns are being returned from the original query.
> - Which columns returned from the original query are of a suitable data type to hold the results from the injected query.

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

> [!INFO]
> We use `NULL` as the values returned from the injected `SELECT` query because the data types in each column must be compatible between the original and the injected queries. `NULL` is convertible to every common data type, so it maximizes the chance that the payload will succeed when the column count is correct.

> [!attention] NullPointerException
> The effect on the HTTP response depends on the application's code. If you are lucky, you will see some additional content within the response, such as an extra row on an HTML table. Otherwise, the null values might trigger a different error, such as a `NullPointerException`. In the worst case, the response might look the same as a response caused by an incorrect number of nulls. This would make this method ineffective.