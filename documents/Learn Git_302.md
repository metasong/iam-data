# Learn Git
---
author: Jason Song <metaseed@gmail.com>
version: 1.0.0
tag: []
subPage: [178]
enable: [toc]

---

http://gitready.com

https://blog.osteele.com/2008/05/my-git-workflow/

http://onlywei.github.io/explain-git-with-d3/#commit

https://marklodato.github.io/visual-git-guide/index-en.html

http://ndpsoftware.com/git-cheatsheet.html
http://justinhileman.info/article/git-pretty/git-pretty.png
https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf
https://github.com/git/git/blob/master/Documentation/howto/revert-a-faulty-merge.txt

[45 个 Git 经典操作场景，专治不会合代码](https://mp.weixin.qq.com/s?__biz=MzA4MjEyNTA5Mw==&mid=2652592303&idx=1&sn=aa690c9c0e9d6bafa5d8c932c88af887&chksm=846570e5b312f9f3938272ba15ce3844c6f6e5044e6bb8963a0c217ba2fe80260e491e3cc336&mpshare=1&srcid=0406bti2kOXt2fw0Qb4I7rSR&sharer_sharetime=1649248962263&sharer_shareid=1eddcb5dbe4634b935d8a5db78a7f438&from=singlemessage&scene=1&subscene=10000&clicktime=1649342458&enterid=1649342458&forceh5=1&ascene=1&devicetype=android-31&version=28001240&nettype=WIFI&abtest_cookie=AAACAA%3D%3D&lang=en&exportkey=AXcGPJVx806MP4bSNao96Do%3D&pass_ticket=IcZWZs1sMT7YSPG%2Ba9WxMw2lh8t5VdvSf0DA0Dxk54mhPYnaiUwrKFefcczyDbKh&wx_header=3)

## Diagrams

![](https://marklodato.github.io/visual-git-guide/basic-usage.svg)
* git reset -- files copies files from the latest commit to the stage. Use this command to "undo" a git add files. 
* git checkout -- files copies files from the stage to the working directory. Use this to throw away local changes.
* You can use git reset -p or --patch, git checkout -p, or git add -p instead of (or in addition to) specifying particular files to interactively choose which hunks copy.

![](https://marklodato.github.io/visual-git-guide/basic-usage-2.svg)

## help
* `git help add` open online help
* `git add -h` show help in console

## file status
![](https://git-scm.com/book/en/v2/images/lifecycle.png)
`git add` is used to stage changes for next commit.
## workflow
![=80%x](https://i.stack.imgur.com/MgaV9.png)

* `git add -u` is update tracked files, untracked file(newly added file) is not included.
* The `git add` command can be used to add ignored files with the -f (force) option

The index is a mechanism for preventing what you commit from matching what you tested in your working directory

The Index is the staging area in git. It’s basically a layer that separates your working tree from the Git repository. This gives developers more power over what gets sent to the Git repository.

HEAD is a pointer that points to the current branch. A repository only has 1 active HEAD.
head is a pointer that points to any commit. A repository can have any number of heads.

![](https://images.osteele.com/2008/git-workflow.png)

This way I can checkpoint every few minutes. It’s a very cheap operation, and I don’t have to spend time cleaning up the checkpoints later. `git diff` tells me what I’ve changed since the last checkpoint; `git diff head` shows what’s changed since the last commit. `git checkout .` reverts to the last checkpoint; `git checkout head .` reverts to the last commit. And `git stash` and `git checkout -m -b` operate on the changes since the last 

https://git-scm.com/book/en/v2/Git-Internals-Git-Objects

commit, which is what I want.