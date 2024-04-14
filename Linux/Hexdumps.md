**Hexdumps** are often used when we want to look at data that cannot be represented in strings and therefore is not readable, so it is easier to look at the hex values. 

A hexdump has three main columns. 
1) The first shows the address, 
2) The second the hex representation of the data on that address and 
3) The last shows the actual data as strings (with ‘.’ being hex values that cannot be represented as a string). 

Many hex editors exist just pick the one you like most.

[[xxd]] can be used to create hexdumps

Hexdumps can be used to figure out the type of a file. 

Each file type has a **magic number/file signature**. You can find [lists](https://en.wikipedia.org/wiki/List_of_file_signatures) with a collection of these different file signatures online. 

The `file` command, which was introduced in [Level 5](https://mayadevbe.me/posts/overthewire/bandit/level5/) also uses this method (and more beyond that). 

This is especially important to know because sometimes files might not have the correct or any file ending to identify its type.