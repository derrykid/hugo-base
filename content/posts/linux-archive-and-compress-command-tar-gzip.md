---
title: "Linux Archive and Compress Command Tar Gzip"
date: 2022-11-04T20:05:41Z
tags: ["command", "tar", "compress", "archive", "gzip"]
categories: ["linux"]
---

## Archive vs compress
As a former Windows user, I didn't have much knowledge about file archiving and file compression. Like most PC users do, I simply collect the files we want to compress and right click the folder to compress then. The output compressed files are, without a doubt, mostly likely to be extension `.zip` or `.rar` files. However, when I switch to Linux, things changed. As on the road of knowing more commands, we inevitably go across files extensions ending like `.tar, .gz`, or `.tar.gz.`

Here, I will explain these mostly known commands for creating archiving and compressing files. Bookmark this article for future use.


## Reasons for user who need to learn these commands

Most people aren't system administrator, but we might want to use these commands because when

1. Re-install operation system, backup files
2. Download resources from Internet and want to extract files
3. Send your files to other users
4. Backup



## TAR - archive files into one `.tar` file. Specify a flag `z` when you want to compress.

File ends with `.tar` (not a compressed file) or `.tgz` (been compressed).

TAR, short for *tape archive*, reveals that it's a tool for backing up tapes. We can use it to create a archive that consists of a group of separate files, separate directories, or a mixture of both.

Without further ado, let's jump in.

Tar option 1 : Create archive. End with `.tar` not compressed. **archive.tar** is the file name you want. Please add `.tar` extension you give the name to the file
```
tar -cvf archive.tar file1.txt ~/Download ~/Documents/cool_stuff/ ...

# c - create.
# f - file
# v - verbose
```

Notice that, we can archive files, and even directories.

Tar option 2 : Add `z` flag (call gzip tool to compress) if you want to compress it. Add `.gz` extension to it.
```
tar  -czvf archive.tar.gz file1.txt file2.txt ...
```

###  Compress individual file in one command

From the `tar` command part we see above, we know we can call these tools by add flags `z` for gzip, and `j` for bzip2.

This part is for those who wants to compress one single archive file or individual file user.

```bash
$ ls
file01.txt  file02.txt
$ gzip file01.txt file02.txt 
$ ls
file01.txt.gz  file02.txt.gz
```
By this example, we understand that `gzip` program will replace the original file. It's simple and powerful.


### decompress by adding a `x` for `tar` command

We can decompress it simply by:

```
tar -xvf archive.tar.gz

# With or without it, still works
tar -xzvf archive.tar.gz  // z option for gzip program
tar -xjvf archive.tar.gz  // j option for bzip2 program
```

We can add `z` flag or `j` flag to it. But it's unnecessary since shell system will detect it for us.

<br/>

Further information **(You can skip this part)** 

Above we see we can add a `z` flag to compress it into `.tar.gz` file. Alternatively, we can call another tool to compress it into file that ends with `.tar.gz2`. We do this by replacing `z` flag with `j` flag. Here is simple example.

```
tar -cjvf archive.tar.bz2 file1.txt file2.txt ...
```


### decompress with `gzip`, `gunzip`, `bunzip2`

For decomposing the `.gz` file, simply add `-d` flag to it. Alternatively, we can use `gunzip`.

```
gzip -d file01.txt.gz file02.txt.gz

# Alternative
gunzip file01.txt.gz file02.txt.gz
```

For `bzip2`, just replace `gzip` command with it.
```
$ bzip2 file01.txt file02.txt 
$ ls
cs2  file01.txt.bz2  file02.txt.bz2

#  uncompress works the same way
[derry@x1 cs]$ bunzip2 file01.txt.bz2
```

## What's the difference between `tar -czvf` and `gzip` ?

`tar -czvf` is shorthand to create archive and compress.

Let's see one example, then you will understand it. We have 2 files: file01.txt and file02.txt. 

We create an archive and compress it. Only with `tar` command.
```
tar -czvf file.tar.gz file01.txt file02.txt
```

Simple and fast.

However, alternatively, you can use this way:
```
# Without z flag
tar -cvf file.tar file01.txt file02.txt

# then compress it with gzip again

gzip file.tar

# At last, we list our directory.
[derry@x1 cs]$ ls
file01.txt  file02.txt  file.tar.gz
```

So! The output is the same! This troubles me a lot before I wrote this article as well. By simply adding a `z` flag, save us a lot of time!
