## What is blind SQL Injection?

Blind SQL injection occurs when an application is vulnerable to SQL injection, but its HTTP responses do not contain the results of the relevant SQL query or the details of any database errors.

Many techniques such as [[UNION SQLi]] are not effective with blind SQL injection vulnerabilities. This is because they rely on being able to see the results of the injected query within the application's responses. 

It is still possible to exploit blind SQL injection to access unauthorized data, but different techniques must be used.

---
## Boolean-based Blind SQLi

Consider an application that uses tracking cookies to gather analytics about usage. Requests to the application include a cookie header like this:

```http
Cookie: TrackingId=u5YD3PapBcR4lN3e7Tj4
```

When a request containing a `TrackingId` cookie is processed, the application uses a SQL query to determine whether this is a known user:

```sql
SELECT TrackingId FROM TrackedUsers WHERE TrackingId = 'u5YD3PapBcR4lN3e7Tj4'
```

This query is vulnerable to SQL injection, but the results from the query are not returned to the user. 

However, the application does behave differently depending on whether the query returns any data. If you submit a recognized `TrackingId`, the query returns data and you receive a "Welcome back" message in the response.

This behavior is enough to be able to exploit the blind SQL injection vulnerability. You can retrieve information by triggering different responses conditionally, depending on an injected condition.

> [!example] Example 1. Blind SQL with Cookie TrackingId
> To understand how this exploit works, suppose that two requests are sent containing the following `TrackingId` cookie values in turn:
> 
> ```sql
> …xyz' AND '1'='1 
> …xyz' AND '1'='2
> ```
> 
> - The first of these values causes the query to return results, because the injected `AND '1'='1` condition is true. As a result, the "Welcome back" message is displayed.
> - The second value causes the query to not return any results, because the injected condition is false. The "Welcome back" message is not displayed.
> 
> This allows us to determine the answer to any single injected condition, and extract data one piece at a time.

> [!example] Example 2. Blind SQL against Password in DB
> For example, suppose there is a table called `Users` with the columns `Username` and `Password`, and a user called `Administrator`. You can determine the password for this user by sending a series of inputs to test the password one character at a time.
> 
> To do this, start with the following input:
> 
> ```sql
> xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 'm'
> ```
> 
> This returns the "Welcome back" message, indicating that the injected condition is true, and so the first character of the password is greater than `m`.
> 
> Next, we send the following input:
> 
> ```sql
> xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) > 't'
> ```
> 
> This does not return the "Welcome back" message, indicating that the injected condition is false, and so the first character of the password is not greater than `t`.
> 
> Eventually, we send the following input, which returns the "Welcome back" message, thereby confirming that the first character of the password is `s`:
> 
> ```sql
> xyz' AND SUBSTRING((SELECT Password FROM Users WHERE Username = 'Administrator'), 1, 1) = 's'
> ```
> 
> We can continue this process to systematically determine the full password for the `Administrator` user.

> The `SUBSTRING` function is called `SUBSTR` on some types of database. For more details, see the SQL injection cheat sheet.

---
## Blind SQLi with conditional errors (Error based)
> (Here when you get an error, that means your query was true)

Some applications carry out SQL queries but their behavior doesn't change, regardless of whether the query returns any data. The technique in Boolean-based [[Blind SQLi]] won't work, because injecting different boolean conditions makes no difference to the application's responses.

It's often possible to induce the application to return a different response depending on whether a SQL error occurs. 

You can modify the query so that it causes a database error only if the condition is true. Very often, an unhandled error thrown by the database causes some difference in the application's response, such as an error message. This enables you to infer the truth of the injected condition.

> [!example] Error-based blind SQLi
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

Next 2 examples are of Oracle DB:

> [!important] Password Length Check
> Checking for password length in DB using Error-based blind SQLi
> ```sql
> '||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'
> ```

> [!important] Password Brute Force Checking
>  Checking first letter of password from a specific user
> ```sql
> '||(SELECT CASE WHEN SUBSTR(password,1,1)='a' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
> ```

Check out [this video](https://youtu.be/HjXUtCKm1FM) by z3nsh3ll to learn more on conditional error based blind SQLi


#security/web/sql 
