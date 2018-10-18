# Learn Git
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
enable: [toc]
---
https://blog.osteele.com/2008/05/my-git-workflow/

## help
* `git help add` open online help
* `git add -h` show help in console

## file status
![](https://git-scm.com/book/en/v2/images/lifecycle.png)
`git add` is used to stage changes for next commit.
## workflow
![=80%x](https://i.stack.imgur.com/MgaV9.png)

* `git add -u` is update tracked files
* The git add command can be used to add ignored files with the -f (force) option
The index is a mechanism for preventing what you commit from matching what you tested in your working directory

![](https://images.osteele.com/2008/git-workflow.png)

This way I can checkpoint every few minutes. It’s a very cheap operation, and I don’t have to spend time cleaning up the checkpoints later. `git diff` tells me what I’ve changed since the last checkpoint; `git diff head` shows what’s changed since the last commit. `git checkout .` reverts to the last checkpoint; `git checkout head .` reverts to the last commit. And `git stash` and `git checkout -m -b` operate on the changes since the last commit, which is what I want.