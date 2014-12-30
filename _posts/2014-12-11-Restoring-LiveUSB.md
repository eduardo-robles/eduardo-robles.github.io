---
layout: post
title: Restoring A LiveUSB
date: '2014-12-11'
---

**Restoring A LiveUSB**

Have you ever made a LiveUSB to boot a Linux distro and found when formatting the usb disk space is lost. For example I made a LiveUSB on a 4GB usb drive and when I formatted it on a Windows machine it only made 4MB available. I was stumped for awhile as to why but then I realized there was partitions that needed to be formatted back to a FAT compatible file system. So let me show you how I fully restored a USB drive...

First, plug in your USB drive into your Linux computer.

Second, find where the USB is located. There are many ways you can do this on Linux but one of my trusted command is like so `sudo ls -l /dev/disk/by-id/*usb*` your USB device should something like /dev/sdX where X is your specific drive letter. Remember there should be NO trailing number, for example this is an incorrect choice /dev/sdX1. Notice the trailing number "1" after sdX.

Third, use the `fdisk` command to edit the partions on your USB drive. With the correct location of your USB drive type this into the terminal `fdisk /dev/sdX`. Again make sure you have the correct drive location!!

Fourth, you will be prompted with a submenu to work with the `fdisk` commands. Type `d` to delete a partition and you will be prompted to select a partition type `1` to select the first partition. Type `d` again to delete the next partition, continue this process untill all partitions have been deleted. 

Fifth, type `n` to create a new partition. You will be asked what type of partition you want to create "Primary" or "Secondary", type `p` to make this partition the primary. Then type `1` to make it the first partition in the USB drive.

Sixth, you will be asked how much space you want for the first and last partitions. It is perfectly fine to use the defaults if you are unsure of what to use here.

Seventh, type `w` to write the new partition to the USB drive.

Eigth, make sure your device is unmounted and you can check with this command `umount /dev/sdX1`. You should get a message stating that your device is NOT mounted.

Ninth, finally we can format the device. The command to do this is `mkfs.vfat -F 32 /dev/sdX1`. And that's it your USB drive should now be restored to its original state. 

Conclusion:
Following this guide can help you restore a USB drive back to its original state after using it as a LiveUSB device. When going through this process make sure you have the correct drive location. This is very important because if you use the incorrect location you can do accidently delete partitions on your hard drive!! This toturial is not too dificult and can be accomplished in 15 minutes.

-Eduardo  
