---
layout: post
title: "How to Initiate, add and delete file using git command"
date: 2017-11-22 16:25:06
tag: git
description: Basic git command.
comments: true
---

# initiate a new repository
* Create a new repository on remote without initalize README.md file
* `mkdir /address/your-local-repfile-name` : must has the same name as the remote repository.
* `cd your-local-repfile-name `
* `touch readme.md`
* `git init`
* `git add.`
* `git status`
* `git commit --m "your commit log"`
* `git push`

# Clone a repository to local.
* `git clone git@github.com:username/username.repositoryname`

# add file and sychronize local to remote
* `git add <filename>`
* `git status`
* `git commit --m "log"`
* `git push`

# delete file and sychronize to remote
* `rm localfile`
* `git rm <filename>` or `git rm -r -f "dictionary-name"`
* `git status`
* `git commit --m "log"`
* `git push`

<hr>

_CopyRight &copy;Newlifehaonan.com_
