### `git (command)` — Used for [[Git (Version Control System)]]

**OPTIONS**
- `git init`= Initializes a new Git repository.
- `git clone <repository_url>`= Downloads a remote repository to your local machine.
- `git add <file>`= Stages changes for commit.
	- `-a` = Add all files
	- `-f` = Forces files to be able to be committed, even when they are normally ignored.
- `git commit`= Creates a new commit.
	- `-a` = Make sure all modified/deleted files are staged
	- `-m "Commit message"` = Specify commit message
- `git status`= Shows working directory status.
- `git log`= Displays commit history.
- `git branch`= Lists branches.
- `git checkout <branch_name>`= Switches to a different branch.
- `git pull`= Fetches and merges changes from a remote repository.
- `git push`= Uploads local commits to a remote repository
	- `-u` = Define the branch (Use when pushing for the first time)
