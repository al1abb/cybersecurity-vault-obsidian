In the previous labs, you used the query string to inject your malicious SQL payload. However, you can perform SQL injection attacks using any controllable input that is processed as a SQL query by the application. For example, some websites take input in JSON or XML format and use this to query the database.

These different formats may provide different ways for you to obfuscate attacks that are otherwise blocked due to WAFs and other defense mechanisms. Weak implementations often look for common SQL injection keywords within the request, so you may be able to bypass these filters by encoding or escaping characters in the prohibited keywords. For example, the following XML-based SQL injection uses an XML escape sequence to encode the `S` character in `SELECT`:

```xml
<stockCheck> 
	<productId>123</productId> 
	<storeId>999 &#x53;ELECT * FROM information_schema.tables</storeId> </stockCheck>
```

This will be decoded server-side before being passed to the SQL interpreter.

> [!example] Lab Example. SQLi in stock check API with XML as parameters
> In lab, we see that the stock check feature works by sending the parameters in xml format.
> Injecting SQLi to any of the parameters does not work and attack is found by WAF(web app firewall). To bypass we decode our SQLi using Hackvertor Extension:
> **Extensions > Hackvertor > Encode > dec_entities/hex_entities**.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<stockCheck>
	<productId>1</productId>
	<storeId>1 
	<@hex_entities>UNION SELECT username||'~'||password FROM users<@/hex_entities>
	</storeId>
</stockCheck>
```
