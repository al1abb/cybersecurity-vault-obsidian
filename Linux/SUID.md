**Suid** (Setuid) is a special permission. It will replace the `x` of the user permission. (rwx => rws)
It means the **==binary will be run as the owner of the binary, not the one executing it==**. 
To give a binary suid permissions the following command needs to be used: 
`chmod u+s <filename>` [[chmod]]