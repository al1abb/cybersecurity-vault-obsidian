“**Git** is a free and open source distributed version control system”
It allows you to save your code, as well as the history and changes you have done to your code. It also makes collaboration and working in teams on the same code simpler.

The Git system contains a lot of commands. Some of the most essential commands are:

- `git init`, to create a new Git repository/project
- `git clone`, to copy an existing git repository
- `git push`, updates remote repository
- `git pull`, get updates from remote repository. 
- `git log`, shows us the commit log.
- `git show <commit>`, shows us the content of a commit

> When creating a public repository it is important to be aware of the information you push to it since changes and previous versions are saved. So sensitive data, like passwords, could still be retrieved

It is also possible to use a Git client that has a GUI for easier interaction ([https://git-scm.com/downloads/guis)](https://git-scm.com/downloads/guis)).

Different services are providing you with a way to host your repository remote (public or private). If you are interested in open-source, these are often also the places to get the software or contribute. Very famous ones would be [Github](https://github.com/) and [GitLab](https://about.gitlab.com/).

The ‘**.git**’ directory [[git (directory)]] contains all information that is required for version control. It contains information about commits, the remote repository address, a log and more.

The **README** [[README]] file is often found in a git repository. It is used as an overview of all the files in a directory or the git project. When creating a git project, a README file is helpful to remember what the project is about, as well as other information that users and developers might need. For example, a short explanation of the project, configuration and installation instructions, licensing information and more.

> [!example]
> Git Clone from SSH on a specific port:
> ````bash
> git clone ssh://user@host:port/home/some-location/repo.git
> ````
> Example
> ````bash
> git clone ssh://bandit27-git@localhost:2220/home/bandit27-git/repo.git
> ````

### **Git branching**

**Git branching** is another feature of the version control system. It allows you to split the development into different branches. Specifically, there is a master branch from which the software can be taken and it can be separately worked on. You can change and add features while still maintaining a working master branch. Once the work is done, it can be integrated into the master branch again. This allows for additional version control. You can offer a production branch with usable software, while fixing bugs or adding features in a different development branch.

The basic commands for working with branches are:

- `git branch`: List (`-a`), create, or delete branches
- `git checkout <branch_name>/git switch <branch_name>`: Switch branches
- `git merge`: Join two or more branches

### **Git Tagging**

**Git tagging** is a way to mark specific points in the history of the repository. One example would be to mark release points of the software. 

The basic commands for working with tags are:

- `git tag`: See tags
- `git tag -a <tag_name> -m <"tag description/message">`: Create a tag
- `git show <tag_name>`: See more details (tag message and commit)