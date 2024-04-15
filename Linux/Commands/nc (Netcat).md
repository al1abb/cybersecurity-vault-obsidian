### `nc` — Allows to read and write data over a network connection.
It can be used for TCP and UDP connections.

**Default Usage**
	`nc <host> <port>` 

**OPTIONS**
- `-l` = Create a server that listens to incoming packets
- `-p` = To specify a port
- C

> [!info]
> To create a “onetime server”, a server that sends one message and then disconnects, we can use the pipe (`|`) and `echo` to input the message.