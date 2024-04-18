If a user can gain access to functionality that they are not permitted to access then this is vertical privilege escalation. For example, if a non-administrative user can gain access to an admin page where they can delete user accounts, then this is vertical privilege escalation.

### Unprotected Functionality
At its most basic, vertical privilege escalation arises where an application does not enforce any protection for sensitive functionality. For example, administrative functions might be linked from an administrator's welcome page but not from a user's welcome page. However, a user might be able to access the administrative functions by browsing to the relevant admin URL.

For example, a website might host sensitive functionality at the following URL:

`https://insecure-website.com/admin`

This might be accessible by any user, not only administrative users who have a link to the functionality in their user interface. 

> In some cases, the administrative URL might be disclosed in other locations, such as the `robots.txt` file:
> 
> `https://insecure-website.com/robots.txt`

Even if the URL isn't disclosed anywhere, an attacker may be able to use a wordlist to brute-force the location of the sensitive functionality.

In some cases, sensitive functionality is concealed by giving it a less predictable URL. This is an example of so-called **"security by obscurity"**. However, hiding sensitive functionality does not provide effective access control because users might discover the obfuscated URL in a number of ways.

Imagine an application that hosts administrative functions at the following URL:

`https://insecure-website.com/administrator-panel-yb556`

This might not be directly guessable by an attacker. However, the application might still leak the URL to users.