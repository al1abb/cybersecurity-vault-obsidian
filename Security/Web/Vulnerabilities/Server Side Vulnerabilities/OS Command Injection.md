OS command injection is also known as shell injection. It allows an attacker to execute operating system (OS) commands on the server that is running an application, and typically fully compromise the application and its data. Often, an attacker can leverage an OS command injection vulnerability to compromise other parts of the hosting infrastructure, and exploit trust relationships to pivot the attack to other systems within the organization.

After you identify an OS command injection vulnerability, it's useful to execute some initial commands to obtain information about the system.

In this attack, the attacker-supplied operating system commands are usually executed with the privileges of the vulnerable application. Command injection attacks are possible largely due to insufficient input validation.

> [!example]
> Imagine a textbox in a website connected to the shell. Lets say it executes this command:
> ````bash
> grep -i $input dictionary.txt
> ````
> 
> We can modify our input so that our own command is injected instead:
> ````bash
> grep -i $input; ls # dictionary.txt
> 			  ^    ^
> 			start end
> ````

#security/web 