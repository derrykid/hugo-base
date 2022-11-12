---
title: "Git How to Switch From Password Prompt to Ssh"
date: 2022-11-12T18:47:19Z
tags: ["ssh", "git"]
categories: ["programming", "git", "linux"]
---

GitHub has terminated the login from password in command line. You can switch to SSH. This tutorial will guide you how to do it.

## Git switch from password to SSH 

What I do is switch remote URLs from HTTPS to SSH. By doing this, the password prompt is no longer there and I can push directly.

Usually we use `git clone https://github.com/USERNAME/repo.git` to get the clone the remote repo to local machine. It's one of the easiest way.

But whenever we want to `git push` to the repo, it's asked for our username and personal access authentication, token. We can switch from HTTPS to SSH.

1. List existing remotes
```bash
git remote -v
> origin  https://github.com/USERNAME/REPOSITORY.git (fetch)
> origin  https://github.com/USERNAME/REPOSITORY.git (push)
```

2. Change remote's URL from HTTPS to SSH with `git remote set-url` command. It's available at repository clone button.
```bash
$ git remote set-url origin git@github.com:USERNAME/repo.git
```

3. Verify by using the commands again:
```
$git remote -v
> origin  git@github.com:USERNAME/REPOSITORY.git (fetch)
> origin  git@github.com:USERNAME/REPOSITORY.git (push)
```
