---
title: "Git - What Is Origin in Git Push Origin Main"
date: 2022-11-12T18:52:09Z
tags: ["git", "command", "linux", "github"]
categories: ["technology and software development"]
---

## What is `origin` in `git push origin main`?

The `origin` is a convention. It's an alias, short for what we see from the output of `git remote -v`. You can name it whatever you want. You can think of it as where this code is "originated" from.
```bash
git remote -v

# output
origin	git@github.com:derrykid/workongit.git (fetch)
origin	git@github.com:derrykid/workongit.git (push)
```


## Set the remote URL as origin and upstream

> Whenever we pull the code from remote repository, it flows downstream. If we push up to remote, it's *upstream.* 

So, if our repository doesn't specify which remote branch as the *upstream.* We have to specify it by `-u`

```bash
git push -u origin main
```

## Set remote URL
```
git remote add origin https://github.com/USERNAME/repo.git

# push changes
git push origin main
```

