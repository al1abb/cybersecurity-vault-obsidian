Linux has these Variables:
- Local variables (Valid in current shell)
- Shell variables (Set up by shell)
- Environment variables (Valid systemwide)

These variables have their names in uppercase only. 
They are defined by writing `VAR_NAME=var_value` in the command line. 
To see the content of a variable, you can write `echo $VAR_NAME`.

Some common that are good to know are:
- `TERM` -  current terminal emulation
- `HOME` - the path to home directory of currently logged in user
- `LANG` - current locales settings
- `PATH` - directory list to be searched when executing commands
- `PWD` - pathname of the current working directory
- `SHELL`/`0` - the path of the current user’s shell
- `USER` - currently logged-in user

> [!info]
> Variable `$0` has reference to a shell. You can confirm this by using `echo $0`
