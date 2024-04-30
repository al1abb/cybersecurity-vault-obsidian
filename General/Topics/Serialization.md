## What is Serialization?

Serialization is the process of converting data structures or objects into a format that can be easily stored, transmitted, or reconstructed later. It's a fundamental concept in programming, especially when dealing with data persistence, network communication, or inter-process communication.

This cuts down the storage size needed and makes it easier to transfer information over a network.

> [!info] Modern frameworks
> Modern frameworks do Serialization behind the scenes

### Serialization Example with Code

Lets say you have a class, you create a string that represents the data in that class and store that string somewhere. And next time you can use the string to go back to the object

Example PHP code
```php
<?php

class User {
	var $firstname;
	var $lastname;
	
	function __construct($firstname = "", $lastname = "")
	{
		$this->firstname = $firstname;
		$this->lastname = $lastname;
	}
	
	function __toString()
	{
		$this->firstname." ".$this->lastname;
	}
}

// Extra logic
// User info is gathered from DB based on SessionID
// User class is generated
$user = new User("Mehmet", "INCE");
echo $user;

```

Commented part is also part of logic. (It was not written to save time)

If you were to run `echo $user;` it would print out the full name based on the `__toString()` method and the commented parts of the code + `$user` declaration would be re run.

To prevent this we use serialization

```php
$store_somewhere = serialize($user);
echo $store_somewhere;
```

Above code returns this result (Contents of `$store_somewhere`)

```bash
O:4:"User":2:{s:9:"firstname";s:6:"Mehmet";s:8:"lastname";s:4:"INCE";}
```

> [!info] Idea of serialization
> Serialization allows you to save often used data (like objects) to Redis DB for fast usage.

---
## Deserialization

Serialization can also be reversed. This is called Deserialization

```php
// deserialize.php

// Require needed class (User class)
require_once("user.class.php");

$serialized_str = 'O:4:"User":2:{s:9:"firstname";s:6:"Mehmet";s:8:"lastname";s:4:"INCE";}';

$user = unserialize($serialized_str);

echo $user;
```

We would get this result if we were to run above code
```bash
Mehmet INCE
```

> [!attention] Require class for deserialization
> Deserialization does not work if the appropriate class is not imported

---
## Destruct method

PHP classes also have `__destruct` magic method that destroys the object after finishing its work with it

```php
// user.class.php

<?php

class User {
	var $firstname;
	var $lastname;
	
	function __construct($firstname = "", $lastname = "")
	{
		$this->firstname = $firstname;
		$this->lastname = $lastname;
	}
	
	function __toString()
	{
		$this->firstname." ".$this->lastname;
	}
	
	function __destruct()
	{
		echo "Object Destruction: ". $this->firstname." ". $this->lastname;
	}
}
```

> [!important] `__destruct` is called automatically
> If you were to serialize this code once again, it would also fire destruct method even though we did not call it. Meaning that `__destruct` method is called automatically

```php
// serialize.php

<?php
require_once("user.class.php");

$user = new User("Mehmet", "INCE");

$store_somewhere = serialize($user);
```

---

Some other magic methods are:
1) `__wakeup()`: Is fired when the class is generated

`__wakeup()` runs before `__destruct()`