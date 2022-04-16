---
title: "How to Format Flash Drive"
date: 2022-03-17T12:06:46+08:00
tags: ["Linux", "command", "format"]
categories: [""]
---

# How to format USB (flash drive) in Linux command line?

Flash drives are very commonly used. From time to time, we stored our files, presentations, logs in this drives. It's a convenient way to copy and carry around. Sometimes, we'll like to reformat a flash drive, either to use a different file system or to clean up for friend.

In Linux, there are several graphic software that provides you the same ability. On Ubuntu, or similar distributions, you can open home and search for "Disks". Usually it's pre-installed in those distributions. However, if you want to try to format your device in command line interface. It's not that difficult either!

So let's try it!

For this article, I use a 8 GiB Flash drive. For calculating reason for different approaches, it might only appear 7.4 GiB sometimes.


## Identify the device

After plugin your device, say flash drive, run the `df` command to see what's the new device that attached to your system.

```
df
```

Alternatively, you can try `lsblk` command to list all the devices.

My output is like the following
```bash
NAME   MAJ:MIN RM   SIZE RO TYPE MOUNTPOINTS
loop0    7:0    0  61.8M  1 loop /var/lib/snapd/snap/core20/1169
loop1    7:1    0  65.2M  1 loop /var/lib/snapd/snap/gtk-common-themes/1519
loop2    7:2    0 242.3M  1 loop /var/lib/snapd/snap/gnome-3-38-2004/76
loop3    7:3    0 124.7M  1 loop /var/lib/snapd/snap/mysql-workbench-community/9
loop4    7:4    0  32.3M  1 loop /var/lib/snapd/snap/snapd/13170
loop5    7:5    0  61.8M  1 loop /var/lib/snapd/snap/core20/1081
loop6    7:6    0     4K  1 loop /var/lib/snapd/snap/bare/5
loop7    7:7    0  32.4M  1 loop /var/lib/snapd/snap/snapd/13640
sda      8:0    0 238.5G  0 disk 
├─sda1   8:1    0    50M  0 part 
├─sda2   8:2    0  32.6G  0 part 
├─sda3   8:3    0   498M  0 part 
└─sda4   8:4    0 205.4G  0 part /
sdb      8:16   1   7.4G  0 disk 
└─sdb1   8:17   1   7.4G  0 part 
```

### Manipulate device partition with `fdisk`

After you identify your device, we can use fdisk command to manipulate the partition.

**fdisk** command is the program that allow us to interact directly with disk-like devices. It can edit, delete, and also create partitions.

First we have to umount the device in order to run it safely.

```
sudo umount /dev/sdb
sudo fdisk /dev/sdb
```

We now will see the following prompt:
```
Command (m for help):
```

Enter "m" for more information.

I shorten this list. Only shows most frequently used ones:

```
......
  Generic
   d   delete a partition
   F   list free unpartitioned space
   l   list known partition types
   n   add a new partition
   p   print the partition table
   t   change a partition type
   v   verify the partition table
   i   print information about a partition


  Save & Exit
   w   write table to disk and exit
   q   quit without saving changes
```

Entering 'p' to show the device partition tables
```
Command (m for help): p
Disk /dev/sdb: 7.44 GiB, 7990149120 bytes, 15605760 sectors
Disk model: Flash Disk      
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x2fe809d7

Device     Boot Start      End  Sectors  Size Id Type
/dev/sdb1        2048 15605759 15603712  7.4G 83 Linux

Command (m for help): 

```

My device has been formatted so it might looks a little different from yours.

Your device type might be **FAT32** or any others.

Now run 'd' to delete the all partition. If you have multiple partitions, you need to run this commands multiple times.

```
Command (m for help): d
Selected partition 1
Partition 1 has been deleted.
```

You can check with command "p" again to see that table has changed.

Now run "n" for add a new partition

```
Command (m for help): n
Partition type
   p   primary (0 primary, 0 extended, 4 free)
   e   extended (container for logical partitions)
Select (default p):
```

We can just hit enter for its default **primary partition** 

Then we'll be prompted with the partition number, sectors, and etc.
```
Partition number (1-4, default 1): 
First sector (2048-15605759, default 2048): 
Last sector, +/-sectors or +/-size{K,M,G,T,P} (2048-15605759, default 15605759): 

Created a new partition 1 of type 'Linux' and of size 7.4 GiB.
Partition #1 contains a ext4 signature.

Do you want to remove the signature? [Y]es/[N]o: y

Command (m for help): 

```

Since my device is 8 GB flash drive, I will simply create one partition from its first default sector to the last default sector. If you want to create more partitions, you can run "n" command multiple times to achieve your goals. 

The **signature**  is about file system. It's a protocol set of constant numerical and text values used to identify file format.

I've removed the signature because **later**  I will use `mkfs` command to create a file system on my flash drive

Finally, we run "t" to change a partition's system id. I use 83 to mark it *Linux*.

### Empty/format the flash drive by `dd` command

Be aware! The dd command is very powerful. Please double check before you run this command. Though its name is "data definition", sometimes users called it "destroy disk". You can tell how powerful this command is to your disk.

dd command syntax is:

```
dd if=input_file of=output_file [bs=block_size [count=blocks]]
```

**if** stands for input file, while **of** stands for output file. Please check again, in case you write into wrong device.


I run the following command to format the disk

```
sudo dd status=progress if=/dev/zero of=/dev/sdb bs=4k && sync
```

**Explanation:** status=progress shows the progress running.

Our input file is /dev/zero. It's a file that contains as many null characters as are read from it. You can use "less" command to check this file, and you'll get stuck in it. 

So we use /dev/zero as input to format our device. Once it finished, it will show

```
dd: error writing '/dev/sdb': No space left on device
```

This indicates it's completed. You don't have to umount it, because we've done it before.  Simply unplug your device. 


### Use `mkfs` command to create file system

After formatting our device, we can create file system. Plug in device again, and also umount the device before `mkfs` command.

```
sudo umount /dev/sdb
```

We can format drives using any of the following: **ext4, vfat, etc** by specifing the type.

Remember to add "number" to your device. It's the partition number. Mine is "1" appended "/dev/sdb1" because I only created one primary partition while we use fdisk command above.

For example I create ext4 file system for my device:

```
sudo mkfs -t ext4 /dev/sdb1
```

After a while, system will finish manipulating your device. Now you have a brand new, formatted device!
