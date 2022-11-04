---
title: "Git Cheatsheet"
date: 2022-06-27T18:47:30+08:00
tags: ["git"]
categories: ["backend"]
---

I learn git from this interactive tutorial. I take note along the way while I was doing it. 

[Learn git visual](https://learngitbranching.js.org) 

## Commit to repo
**We was on C1 commit** 

```bash
git commit
```
![commit](./img/commit.png) 

## Branch

### new branch 

```bash
git branch <name>
```
![branch new](./img/create-branch.png) 

### change branch 

```bash
git checkout <name>
```

### create a new branch and check it out

It can be shorthand:
```bash
git checkout -b <name>
```

## merge

**Now we are working on main branch** 

We are going to merge the branch `bugFix` into `main`

```bash
git merge bugFix
```

![merge](./img/merge.png) 

## rebase

**Note that the branch we are on, will 'change the base' to another branch.** 

Request: Move work directly from bugFix onto `main` branch 

**We are on bugFix branch** 

```bash
git rebase main
```

Before rebase

![git rebase main](./img/git-rebase-main.png) 

After rebase

![git-rebase-main-after](./img/git-rebase-main-after.png) 

Now we know `bugFix` is ahead of `main` branch so we have to merge `main` branch with `bugFix` branch as well

```bash
git rebase bugFix
```

![git rebase bugFix](./img/git-rebase-bugFix.png) 

Since `main` was an ancestor of `bugFix`, `git` simply moved the `main` reference forward in history.

### rebase \<branch1> \<branch2>

**We are on main branch.**

```bash
git rebase main bugFix
```

### rebase interactive `-i`

Open up a text editor, show which commits are about to be copied below the target rebase. It also shows their commit hashes and message.

```bash
git rebase -i HEAD~4
```

## Log

```bash
git log
```

## HEAD

What is HEAD? HEAD is the work in action. Simply put: what you're doing now is the `HEAD`. Once you commit, the HEAD becomes the branch commit you work on.

Let's see where the `HEAD` is hiding:
```bash
git checkout C2
```

![git checkout C2](./img/head.png) 


## relative refs

- Moving upwards one commit `^`
- Moving upwards a number of times `~<num>`


```bash
git checkout main^
```
![git checkout main](./img/git-checkout-main.png) 

Move 2 times 
```bash
git checkout HEAD~2
```

### branch forcing

You can directly reassign a branch to a commit with `-f` flag

**On bugFix branch, main was on C4 commit.** 

```bash
git branch -f main HEAD~3
```

After 

![git branch f](./img/gitbranch-f.png) 

Reassign a branch `main` to a hash commit, e.g. `C6` commit

```bash
git branch -f main C6
```

## Reset

**Works for local machine** 

`git reset` reverses changes by moving a branch reference backwards in time to an older commit, i.e. *rewriteing history*

```bash
git reset HEAD~1
```

![git reset](./img/git-reset.png) 

## Revert

In order to reverse changes and share those reversed changes with others, use `git revert`

```bash
git revert HEAD
```

**A new commit plopped down below the commit we wanted to reverse.** This new commit introduces changes - reverse the commit

![git revert](./img/git-revert.png) 

## git cherry-pick \<Commit1> \<Commit2> \<...>

Copy a series of commits below your current location `HEAD`. **As long as that commit isn't an ancestor of HEAD.** 

Suppose we are on `main`, and we want `C2`, and `C4` commits.

```bash
git cherry-pick C2 C4
```

This will plop down the `main` branch twice

Before

![cherry before](./img/cherry-pick-before.png) 

After 

![cherry after](./img/cherry-pick-after.png) 

## git tag \<tagname> \<Commit>

Tag is used to permanently mark a historical points in the history. It works as an anchor.


```bash
git tag v1 C1
```

![git tag](./img/git-tag.png) 

## git describe \<ref>

output: `<tag>_<numCommits>_g<hash>`, the hash is where the branch on

```bash
git describe main
```

## git clone

`origin/main` is called remote branch. When you check them out, you're put into detached `HEAD` mode. Git does this on purpose because you can't work on these branches directly.

To be clear: Remote branches are on your local repository, not on the remote repository.

## git fetch

Fetch data from the remote repository

For example, when working on your local codebase, the remote repository has updated and committed few times. In order to **download these commits**, you can use `git fetch` to update your `origin/main`. 

**This update our local `remote branch`** into synchronization with what the actual remote repository looks like now. **It doesn't change anything about your local state.** You have to use `git merge origin/main` to merge the code.

```bash
git fetch
```

Don't need to put any other arguments to it.

### git fetch \<source>:\<destination>

Works like `git push origin main` while it's on opposite direction

## git pull

Often, when you `git fetch`, you'd like to merge it with your local `origin/main`:

Use one of the following:

- `git merge origin/main`
- `git rebase origin/main`
- `git cherry-pick origin/main`

```bash
git fetch
git merge origin/main
```

And `git` provides a shorthand for it `git pull`

```bash
git pull
```

![git pull](./img/git-pull.png) 

## git push

`git push` is responsible for uploading your changes to a specified remote and updating that remote to incorporate your new commits. You can think of `git push` as a command to "publish" your work.

```bash
git push
```

### git push \<remote> \<place>

The <place> is what branch locally we like to commit to remote. For example, `git push origin main`, we simply tell `git` that we want the local `main` branch to push to remote origin `main`.

```bash
git push origin main
```

**Note that we don't have to use `origin/main`, simply `origin`** 

### git push origin \<source>:\<destination>

`<source>` is where you specify your commit, e.g. C1, C2, etc.

`<destination>` is the remote branch name

Before git push

![git place](./img/git-push-place1.png) 

```bash
git push origin foo^:main
```

After:

![git place](./img/git-push-place2.png) 


### Real word example of `git push`

![git push1](./img/git-push-1.png) 

The image above, you cannot `git push` because your local repository is on `C3` while the remote repository is on `C2`.

It's common for team project. You work on your allocated part and others work on theirs.

Now, how do we push our work to remote repository?

####  Use rebase

1. git fetch
2. git rebase origin/main
3. git push

The result:

![git push 2](./img/git-push-2.png) 


#### Use merge

```bash
git fetch
git merge origin/main
git push
```

![git push 3](./img/git-push-3.png) 

#### Use `git pull` shorthand

Tired of typing so many commands? Use `git pull` !

`git pull` is shorthand for a fetch and merge. Optionally, you can use `git pull --rebase` for a fetch and a rebase

```bash
git pull --rebase
git push
```

![git push 4](./img/git-push-4.png) 

Alternatively, use merge

```bash
git pull
git push
```

![git push 5](./img/git-push-5.png) 

## remote tracking

Name a different branch to push to remote `main`

```bash
git checkout -b totallyNotMain origin/main
```

Another way to set remote tracking on a branch is to simply use `git branch -u`
```bash
git branch -u o/main totallyNotMain
```

If `totallyNotMain` is checked out, it can be shorter:
```bash
git branch -u origin/main
```
