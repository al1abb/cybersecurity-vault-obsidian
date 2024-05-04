## Basics

Path traversal is also known as directory traversal. These vulnerabilities enable an attacker to read arbitrary files on the server that is running an application. This might include:

- Application code and data.
- Credentials for back-end systems.
- Sensitive operating system files.

In some cases, an attacker might be able to write to arbitrary files on the server, allowing them to modify application data or behavior, and ultimately take full control of the server.

Imagine a shopping application that displays images of items for sale. This might load an image using the following HTML:

`<img src="/loadImage?filename=218.png">`

The `loadImage` URL takes a `filename` parameter and returns the contents of the specified file. The image files are stored on disk in the location `/var/www/images/`. To return an image, the application appends the requested filename to this base directory and uses a filesystem API to read the contents of the file. In other words, the application reads from the following file path:

`/var/www/images/218.png`

This application implements no defenses against path traversal attacks. As a result, an attacker can request the following URL to retrieve the `/etc/passwd` file from the server's filesystem:

`https://insecure-website.com/loadImage?filename=../../../etc/passwd`

This causes the application to read from the following file path:

`/var/www/images/../../../etc/passwd`

The sequence `../` is valid within a file path, and means to step up one level in the directory structure. The three consecutive `../` sequences step up from `/var/www/images/` to the filesystem root, and so the file that is actually read is:

`/etc/passwd`

---
## How to detect Directory Traversal vuln.

Go back one directory and then go inside the directory again
Original  `24.jpg`
Modified `../image/24.jpg`

If it accepts our modified payload, then there is directory traversal

---
## Null byte injection

Imagine this scenario. 

`path = BASE_PATH + 'static' + '/' + folder + '/file/test.txt'`

You can control `folder`.

Your usual directory traversal won't work as there is a different folder that it will go to (`/file/test.txt`)

In old PHP version, there is a vulnerability where you can do this. (`%00` is null byte) 
`../../../../../../../../../../etc/passwd%00`

Null byte (`%00`) removes anything that comes after it.

> [!info] Test for null byte injection
> You can test for null byte injection using this
> `/image?filename=58.jpg%00sometexthere`
> or
> `/image?filename=58.jpg%00sometexthere.jpg`
> 
> Based on app response you can determine if there is a null byte injection vulnerability or no

---
## Important Notes

> [!info] No limit on directory traversal
> Imagine that you are in this directory `/home/carlos/static`.
> Even if you write `../` more than 4-5 times, you will still end up in root directory (`/`)

> [!important] Bypassing input stripping (non-recursively) implemented by developers
> Sometimes, the developers block `../` from directory input by stripping it.
> Ex: `?filename=../24.jpg => ?filename=24.jpg`
> 
> To bypass this you can do this (Nested traversal sequence)
> `?filename=....//24.jpg => ../24.jpg` (`../` from the middle of the filename gets removed but the sides remain)
> 
> There would be a **DOS** vulnerability if the developers tried to recursively strip input in this case!

---
## Prevention

The most effective way to prevent path traversal vulnerabilities is to avoid passing user-supplied input to filesystem APIs altogether. Many application functions that do this can be rewritten to deliver the same behavior in a safer way.

If you can't avoid passing user-supplied input to filesystem APIs, we recommend using two layers of defense to prevent attacks:

- Validate the user input before processing it. Ideally, compare the user input with a whitelist of permitted values. If that isn't possible, verify that the input contains only permitted content, such as alphanumeric characters only.
- After validating the supplied input, append the input to the base directory and use a platform filesystem API to canonicalize the path. Verify that the canonicalized path starts with the expected base directory.

Below is an example of some simple Java code to validate the canonical path of a file based on user input:

```java
File file = new File(BASE_DIRECTORY, userInput); 
if (file.getCanonicalPath().startsWith(BASE_DIRECTORY)) { 
	// process file 
}
```

---
## Related links

Check out this [video](https://youtu.be/wNMyiqixL1g) by mdisec to learn more