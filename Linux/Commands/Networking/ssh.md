### `ssh` — Used to connect to devices remotely

**Default Usage**
	`ssh` 

**OPTIONS**
- `-p` = Port to connect to
- `-i [ssh_private_key]` = allows login with the private key

**SSH**, which is short for _Secure Shell Protocol_. It is used to remotely connect to a machine. As the name suggests, this protocol aims for secure communication between the machines.

When you work with Linux, you can ssh into a machine through a terminal using the _ssh_ command. Like with almost any Linux command, if you want to know more about it and its options, you can use the `man` command (`man ssh`).

With Windows, you can use software like [PuTTY](https://www.putty.org/).


> [!info] SSH remote execution of commands
> SSH also allows remote execution of commands by adding the commands after the common SSH expression:
> ````bash 
> ssh bandit18@bandit.labs.overthewire.org -p 2220 ls 
>  ````
>   
>  Alternatives:
>  ````bash 
>  ssh bandit18@bandit.labs.overthewire.org -p 2220 /bin/bash
>  OR
>  ssh bandit18@bandit.labs.overthewire.org -p 2220 -t /bin/sh


