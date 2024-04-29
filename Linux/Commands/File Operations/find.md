### `find` — Search for files in a directory hierarchy

**Default Usage**
	`find [file_name]` 

**OPTIONS**
- `-size [bytes]` = Look for files in this size
- `-type f` = No directories/non-executables (only files)
- `-name [file_format]` = Finds files with pattern and file matching
	- `-name "*.doc"`
- `-printf [format]` = Specify the format of the output
	- `-printf "%P\n"` = Print relative paths of found files + new line
- `-readable` = Show readable (permission) files
- `! -executable` = Non-executables
- `-exec [command]` = Execute a command
- ` */* ` = Select All files inside all directories (1st star side is dir, 2nd star side is file)
- ` */{.,}* ` = Select All Hidden files inside all directories
- `-user [username]` = Find files owned by a specific user
- `-group [groupname]` = Find files owned by a specific group
- `-mindepth [number]` = Start its work at a certain level of depth within the directory structure
	- `-mindepth 1` = Don't include the current directory itself
- `-maxdepth [number]` = Maximum depth
	- `-maxdepth 1` = Don't descend into subdirectories (only current directory)
- `-delete` = Deletes files and directories found by `find` command
