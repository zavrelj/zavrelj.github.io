---
layout: post
title: How to enlarge and expand Ubuntu virtual machine hard drive
date: '2015-04-04 15:36:00'
tags: linux
---

When you install virtual machine, you have to decide what size of virtual drive to set, but sometimes you might later realize that it was not enough.

In that case you can quite easily enlarge your virtual hard drive and then expand your operating system to newly added free space. This article will show you how to do it with VirtualBox's .vdi file. But there's more. Usually you would need to boot from external drive, but not this time. The whole process can be done from terminal. So let's start.

Shutdown your virtual machine, open terminal on your physical machine, cd into your VirtualBox VMs folder, locate folder with your virtual machine, cd into it and run this command:

	VBoxManage modifyhd YOUR_HARD_DISK.vdi --resize SIZE_IN_MB  
YOUR_HARD_DRIVE will be the name of your .vdi file
SIZE_IN_MB will be new size, like 16000

That's it. Your virtual hard drive is enlarged. Now you need to expand your virtual machine into newly added free space.
Run your Ubuntu virtual machine, open it's terminal and run this command:

	sudo fdisk /dev/sda

Type **p** to get the list of all partitions and make a note of the start cylinder of **/dev/sda1** *(usually 2048)*

Type **d** to delete the swap partition, then delete the **/dev/sda1** partition

Type **n** to create a new primary partition. Make sure it starts at the number you wrote down. For the rest, let the default value

Type **a** to set the bootable flag on your new **/dev/sda1**

Type **w** to write the new partition table to disk

	sudo reboot
	sudo resize2fs /dev/sda1  

That's it! You have expanded your virtual machine. Check the size of your virtual hard drive. It reads 16 GB now.
