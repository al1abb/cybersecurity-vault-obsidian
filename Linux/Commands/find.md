##### Usage

`find`
--
- `-size <bytes>` = Look for files in this size
- `-type f` = No directories/non-executables (only files)
- `-readable` = Show readable (permission) files
- `! -executable` = Non-executables
- `-exec <command>` = Execute a command
- ` */* ` = Select All files inside all directories (1st star side dir, 2nd star side file)
- ` */{.,}* ` = Select All Hidden files inside all directories
- `-user <username>` = Find files owned by a specific user
- `-group <groupname>` = Find files owned by a specific group