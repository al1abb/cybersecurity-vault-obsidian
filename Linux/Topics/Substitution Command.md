`somecommand $(...)` This command is substitution command.

It takes the output of its inside `(...)` and gives it as input to previous command 

> [!example] Substitution Command
> Example with find
> 
> ```bash
> grep -h "500" $(find . -type f -name "access.log*")
> ```