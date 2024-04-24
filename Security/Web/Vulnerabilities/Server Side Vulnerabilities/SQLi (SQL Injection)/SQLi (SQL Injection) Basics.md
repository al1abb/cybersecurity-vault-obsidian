SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database.

In some situations, an attacker can escalate a SQL injection attack to compromise the underlying server or other back-end infrastructure. It can also enable them to perform denial-of-service attacks.

## ⭐ How to detect SQLi vulnerabilities
You can detect SQL injection manually using a systematic set of tests against every entry point in the application. To do this, you would typically submit:

- The single quote character `'` and look for errors or other anomalies.
- Some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and look for systematic differences in the application responses.
- Boolean conditions such as `OR 1=1` and `OR 1=2`, and look for differences in the application's responses.
- Payloads designed to trigger time delays when executed within a SQL query, and look for differences in the time taken to respond.
- OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitor any resulting interactions.
---
## SQL Injections in different parts of the query
Most SQL injection vulnerabilities occur within the `WHERE` clause of a `SELECT` query. Most experienced testers are familiar with this type of SQL injection.

However, SQL injection vulnerabilities can occur at any location within the query, and within different query types. Some other common locations where SQL injection arises are:

- In `UPDATE` statements, within the updated values or the `WHERE` clause.
- In `INSERT` statements, within the inserted values.
- In `SELECT` statements, within the table or column name.
- In `SELECT` statements, within the `ORDER BY` clause.
---
## Retrieving hidden data (Product category example)
Imagine a shopping application that displays products in different categories. When the user clicks on the **Gifts** category, their browser requests the URL:

`https://insecure-website.com/products?category=Gifts`

This causes the application to make a SQL query to retrieve details of the relevant products from the database:

```sql
SELECT * FROM products WHERE category = 'Gifts' AND released = 1
```

This SQL query asks the database to return:

- all details (`*`)
- from the `products` table
- where the `category` is `Gifts`
- and `released` is `1`.

The restriction `released = 1` is being used to hide products that are not released. We could assume for unreleased products, `released = 0`.

The application doesn't implement any defenses against SQL injection attacks. This means an attacker can construct the following attack, for example:

`https://insecure-website.com/products?category=Gifts'--`

This results in the SQL query:

```sql
SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1
```

Crucially, note that `--` is a comment indicator in SQL. This means that the rest of the query is interpreted as a comment, effectively removing it. In this example, this means the query no longer includes `AND released = 1`. As a result, all products are displayed, including those that are not yet released.

You can use a similar attack to cause the application to display all the products in any category, including categories that they don't know about:

`https://insecure-website.com/products?category=Gifts'+OR+1=1--`

This results in the SQL query:

```sql
SELECT * FROM products WHERE category = 'Gifts' OR 1=1--' AND released = 1
```

The modified query returns all items where either the `category` is `Gifts`, or `1` is equal to `1`. As `1=1` is always true, the query returns all items.

> [!warning]
> Take care when injecting the condition `OR 1=1` into a SQL query. Even if it appears to be harmless in the context you're injecting into, it's common for applications to use data from a single request in multiple different queries. If your condition reaches an `UPDATE` or `DELETE` statement, for example, it can result in an accidental loss of data.

---
## Subverting application logic (Login example)
Imagine an application that lets users log in with a username and password. If a user submits the username `wiener` and the password `bluecheese`, the application checks the credentials by performing the following SQL query:

```sql
SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'
```

If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

In this case, an attacker can log in as any user without the need for a password. They can do this using the SQL comment sequence `--` to remove the password check from the `WHERE` clause of the query. For example, submitting the username `administrator'--` and a blank password results in the following query:

```sql
SELECT * FROM users WHERE username = 'administrator'--' AND password = ''
```

This query returns the user whose `username` is `administrator` and successfully logs the attacker in as that user.

---
## Interesting SQL Behaviors

These are taken from MySQL database. Others might work differently

| <div style="width:120px">Statement</div>   | Result                                               | Description                                                                                                                                                                                                                                       |
| ------------------------------------------ | ---------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| SELECT 1;                                  | 1                                                    | Returns the value `1` directly.                                                                                                                                                                                                                   |
| SELECT 2-1;                                | 1                                                    | SQL performs the operation `2-1` and returns the resulting value `1`.                                                                                                                                                                             |
| SELECT 2+1;                                | 3                                                    | SQL performs the addition `2+1` and returns the resulting value `3`.                                                                                                                                                                              |
| SELECT '2-1';                              | 2-1                                                  | Returns the string `'2-1'` as it is enclosed in single quotes.                                                                                                                                                                                    |
| SELECT '2'-'1';                            | 1                                                    | SQL interprets the enclosed strings as numbers and performs subtraction: `2 - 1 = 1`.                                                                                                                                                             |
| SELECT '2'+'1';                            | 3                                                    | SQL interprets the enclosed strings as numbers and performs addition: `2 + 1 = 3`.                                                                                                                                                                |
| SELECT '2'+'a';                            | 2                                                    | String is understood as 0 when adding with a number                                                                                                                                                                                               |
| SELECT 'b'+'a';                            | 0                                                    | 'b' and 'a' are considered as 0                                                                                                                                                                                                                   |
| SELECT '2' '1';                            | 21                                                   | Space joins the separate parts into one.<br><br>Below code returns the same result: <br>`SELECT concat('2', '1')`                                                                                                                                 |
| SELECT '2' '1' 'a';                        | 21a                                                  | Joins(concats) strings `'2'`, `'1'`, and `'a'` using spaces, resulting in the string `'21a'`.                                                                                                                                                     |
| SELECT '2' '1' 'a'-1;                      | 20                                                   | 'a'-1=-1, because 'a' is considered as 0 when calculating with numbers. '2' '1'=21<br><br>Then it evaluates this: 21 - 1, which equals 20                                                                                                         |
| SELECT 2^1;                                | 3                                                    | `^` is XOR operator. 2=0010; 1=0001; 2 XOR 1 = 0011 = 3                                                                                                                                                                                           |
| SELECT 2^2;                                | 0                                                    | 2=0010; 2 XOR 2 = 0000 = 0                                                                                                                                                                                                                        |
| SELECT !1;                                 | 0                                                    | The `!` operator is the logical NOT operator. `!1` evaluates to `0` because `1` is considered `true`, and logical NOT of `true` is `false`, represented as `0`.                                                                                   |
| SELECT ~1;                                 | 18446744073709551614<br>(Or another very big number) | The `~` operator is the bitwise NOT operator. <br>`1` in binary is represented as `00000000000000000000000000000001`. <br>Bitwise NOT of this binary value is `11111111111111111111111111111110`, <br>which in decimal is `18446744073709551614`. |
| SELECT * FROM users WHERE username='admin' | Returns multiple users                               | Imagine you have a database table called users. It has 4 users. Note the spaces after names:<br>'admin', 'admin ', 'admin  ', 'admin   '<br>When selecting, SQL will show all of them<br>To fix this, exact match has to be turned ON             |

---
## SQLi Types

1) [[UNION SQLi]] 

> [!important] UNION SQLi example
> Basic UNION SELECT query. Check [[UNION SQLi]] page for more
> ```sql
> UNION SELECT 1,2,3...
> ```

2) [[Error-based SQLi]]

> [!important] Error-based SQLi example
> Write a custom subquery using extractvalue function and return needed data in error
> ```sql
> SELECT EXTRACTVALUE(rand(), concat(1, (YOUR_SUBQUERY_HERE)))
> ```

3) [[Blind SQLi]] (Boolean and Time based)
4) [[Out-of-band (OAST)]]

---
## SQL Clauses and Functions

|       Clause        |                                   Description                                    |     |      Function      |                                      Description                                      |
| :-----------------: | :------------------------------------------------------------------------------: | :-: | :----------------: | :-----------------------------------------------------------------------------------: |
|     **SELECT**      |                     Retrieves data from one or more tables.                      |     |   **SUBSTRING**    | Extracts a substring from a string based on a specified starting position and length. |
|      **FROM**       |               Specifies the table(s) from which to retrieve data.                |     |     **ASCII**      |          Returns the ASCII code value of the leftmost character of a string.          |
|      **WHERE**      |                  Filters records based on specified conditions.                  |     |  **EXTRACTVALUE**  |        Extracts a value from an XML string using a specified XPath expression.        |
|    **GROUP BY**     |           Groups rows that have the same values in specified columns.            |     |     **LIMIT**      |               Limits the number of rows returned in a query result set.               |
|     **HAVING**      |               Filters group records based on specified conditions.               |     |     **CONCAT**     |                      Concatenates two or more strings together.                       |
|    **ORDER BY**     |                 Sorts the result set based on specified columns.                 |     |     **LENGTH**     |                     Returns the length of a string in characters.                     |
|    **DISTINCT**     |                     Returns unique values in the result set.                     |     |     **UPPER**      |                   Converts all characters in a string to uppercase.                   |
|   **INSERT INTO**   |                        Inserts new records into a table.                         |     |     **LOWER**      |                   Converts all characters in a string to lowercase.                   |
|     **UPDATE**      |                      Modifies existing records in a table.                       |     |    **REPLACE**     |               Replaces all occurrences of a substring within a string.                |
|     **DELETE**      |                          Deletes records from a table.                           |     |      **TRIM**      |                  Removes leading and trailing spaces from a string.                   |
|      **JOIN**       |         Combines rows from two or more tables based on a related column.         |     |     **ROUND**      |               Rounds a number to a specified number of decimal places.                |
|   **INNER JOIN**    |                Returns rows when there is a match in both tables.                |     |      **CEIL**      |                      Rounds a number up to the nearest integer.                       |
|    **LEFT JOIN**    | Returns all rows from the left table, and the matched rows from the right table. |     |     **FLOOR**      |                     Rounds a number down to the nearest integer.                      |
|   **RIGHT JOIN**    | Returns all rows from the right table, and the matched rows from the left table. |     |      **NOW**       |                          Returns the current date and time.                           |
| **FULL OUTER JOIN** |           Returns all rows when there is a match in one of the tables.           |     |  **DATE_FORMAT**   |                              Formats a date as a string.                              |
|      **UNION**      |            Combines the result sets of two or more SELECT statements.            |     |    **CURDATE**     |                               Returns the current date.                               |
|     **EXISTS**      |               Tests for the existence of any record in a subquery.               |     |    **CURTIME**     |                               Returns the current time.                               |
|   **NOT EXISTS**    |             Tests for the non-existence of any record in a subquery.             |     |      **CASE**      |     Evaluates a list of conditions and returns one of multiple possible results.      |
|       **IN**        |                Filters the result set based on a list of values.                 |     |    **COALESCE**    |                      Returns the first non-null value in a list.                      |
|     **NOT IN**      |               Excludes values from the result set based on a list.               |     |      **CAST**      |                    Converts a value from one data type to another.                    |
|     **BETWEEN**     |                      Filters the result set within a range.                      |     |     **NOW()**      |                          Returns the current date and time.                           |
|   **NOT BETWEEN**   |               Excludes values from the result set within a range.                |     |    **DATE_ADD**    |                       Adds a specified time interval to a date.                       |
|      **LIKE**       |                  Searches for a specified pattern in a column.                   |     |    **DATE_SUB**    |                   Subtracts a specified time interval from a date.                    |
|    **NOT LIKE**     |                  Excludes rows that match a specified pattern.                   |     | **TIMESTAMPDIFF**  |                    Returns the difference between two timestamps.                     |
|     **IS NULL**     |                              Tests for null values.                              |     |  **TIMESTAMPADD**  |                    Adds a specified time interval to a timestamp.                     |
|   **IS NOT NULL**   |                            Tests for non-null values.                            |     | **LAST_INSERT_ID** |                       Returns the ID of the last inserted row.                        |
|       **AS**        |                     Renames a column or table with an alias.                     |     |     **RAND()**     |                       Generates a random floating-point number.                       |
|      **AVG()**      |                  Returns the average value of a numeric column.                  |     |    **ROUND()**     |               Rounds a number to a specified number of decimal places.                |
|     **COUNT()**     |           Returns the number of rows that match a specified condition.           |     |     **CEIL()**     |                      Rounds a number up to the nearest integer.                       |
|      **SUM()**      |                    Returns the total sum of a numeric column.                    |     |    **FLOOR()**     |                     Rounds a number down to the nearest integer.                      |
|      **MAX()**      |                Returns the largest value of the selected column.                 |     |     **ABS()**      |                        Returns the absolute value of a number.                        |
|      **MIN()**      |                Returns the smallest value of the selected column.                |     |     **SQRT()**     |                         Returns the square root of a number.                          |
|     **UPPER()**     |                Converts all characters in a string to uppercase.                 |     |    **POWER()**     |                    Raises a number to the power of another number.                    |
|     **LOWER()**     |                Converts all characters in a string to lowercase.                 |     |     **MOD()**      |                         Returns the remainder of a division.                          |
|     **TRIM()**      |                Removes leading and trailing spaces from a string.                |     |  **CONCAT_WS()**   |                        Concatenates strings with a separator.                         |
|      **IF()**       |                      Returns a value based on a condition.                       |     |                    |                                                                                       |


Check out [this video](https://www.youtube.com/watch?v=WtHnT73NaaQ) by mdisec for SQL Injection 101