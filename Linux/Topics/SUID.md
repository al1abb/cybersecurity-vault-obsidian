**SUID** (Setuid) is a special permission. It will replace the `x` of the user permission. (rwx => rws)
It means the **==binary will be run as the owner of the binary, not the one executing it==**. 

> [!Giving SUID permission]
> To give a binary suid permissions the following [[chmod]] command needs to be used: 
> 
> ```bash
> chmod u+s <filename>```
