Insecure direct object references (IDOR) are a type of access control vulnerability that arises when an application uses user-supplied input to access objects directly. 

IDOR vulnerabilities are most commonly associated with [[Horizontal Privilege Escalation]], but they can also arise in relation to [[Vertical Privilege Escalation]].

---
## IDOR vulnerability with direct reference to database objects

Consider a website that uses the following URL to access the customer account page, by retrieving information from the back-end database:

```http
https://insecure-website.com/customer_account?customer_number=132355
```

Here, the customer number is used directly as a record index in queries that are performed on the back-end database. 
If no other controls are in place, an attacker can simply modify the `customer_number` value, bypassing access controls to view the records of other customers. 
This is an example of an IDOR vulnerability leading to horizontal privilege escalation.