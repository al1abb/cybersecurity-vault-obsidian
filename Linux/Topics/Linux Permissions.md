**Linux permissions** is a very big topic. Each file has an owner and a group that owns the file. 

For the user, the group and the rest of the users the permissions can be set separately. It can be chosen if the file is readable, writable or executable. 

The permissions and owners of a file can be seen using the `ls -l`([[ls]]) command. The third column shows the user, the fourth the group and the first column shows the permissions. 

The permissions are written the following `rwxrwxrwx`, the first three letters indicate the user (file owner) has all permissions (read=r, write=w and execute=x). The next three letters indicate the permission of the group and the last of everyone else. If one of the letters is replaced with `-`, this means the permission is not granted.

[[SUID]] is a special permission. [[chmod]] can be used to give a binary SUID permissions.

![[Linux Permissions. RWX.png]]

![[Linux File Permissions. Binary+Octal.png]]
