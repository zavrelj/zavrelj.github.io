---
layout: post
title: Laravel/Homestead - the real installation guide
date: '2015-03-30 06:17:27'
tags: laravel
---

I got stuck when following [Say Hello to Laravel Homestead 2.0](https://laracasts.com/lessons/say-hello-to-laravel-homestead-two) tutorial, because there is one huge omission. Here is the complete step by step guide.

Just **Follow** and then read **Explain** if you want to know more.

### Follow

1. Get a Mac or use Ubuntu on bare metal machine (not virtualized, more on that in Explain section below)

2. Go to [www.vagrantup.com](https://www.vagrantup.com) and hit download *(don't use apt-get on Ubuntu, since it will give you old version 1.0.1 while current version is 1.7.x)*

3. Install vagrant_X.X.X.dmg file *(or .deb file on Ubuntu, where you probably already have some older version of Vagrant installed)*

4. Open terminal

5. Install PHP, CURL and [Composer](https://getcomposer.org)

		sudo apt-get install php5-cli

		sudo apt-get install curl

		curl -sS https://getcomposer.org/installer | php

6. Create these two folders if they don't exist already *(they don't exist in OS X Yosemite, you should already have them in Ubuntu)*

		sudo mkdir /usr/local

		sudo mkdir /usr/local/bin

7. Move composer file to newly created folder

		sudo mv composer.phar /usr/local/bin/composer

   On Ubuntu you will be able to access it globally since **/usr/local/bin** is already in your PATH by default. On Yosemite however, you might need to add **/usr/local/bin** to your PATH like this:

        sudo nano ~/.bash_profile
 and add this line at the end of the file:

        export PATH="/usr/local/mysql/bin:$PATH"

 Write changes **CTRL + O**, **ENTER** to confirm file name and **CTRL + X** to exit nano

8. Install [VirtualBox](https://www.virtualbox.org) on Mac or run this command on Ubuntu:
			sudo apt-get install virtualbox-qt

9. Install Homestead box to Vagrant *(choose virtualbox when asked, this will take a while, so make a coffee)*

		vagrant box add laravel/homestead

10. Install Homestead CLI tool

		composer global require "laravel/homestead=~2.0"

 If you're on Ubuntu 12.04 LTS, you will probably have PHP version 5.3.x which is not enough for Homestead. If this is your case, then you will end up with
**Installation failed, deleting ./composer.json** error.

 To fix this, just run these commands to get PHP 5.5:

        sudo add-apt-repository ppa:ondrej/php5

				sudo apt-get update

				sudo apt-get dist-upgrade

 Then run the first command of this step again.

11. Make homestead executable accessible globally by adding this line to ~/.bash_profile (or /etc/bash.bashrc on Ubuntu):

        export PATH="~/.composer/vendor/bin:$PATH"

	And restart terminal.

12. Create Projects folder in your home folder (default Code folder would work just fine, but we want to have an opportunity to modify our YAML file and this is good reason)

		mkdir ~/Projects

13. Cd into Projects folder and create Homestead.yaml configuration file

		homestead init

	Homestead.yaml will be placed in ~/.homestead directory and you need to edit it:

		homestead edit

	Now you want to change *Code* directory in yaml file to *Projects* directory and lowercase *Laravel* to *laravel* so it looks this way (on Ubuntu there might be more at the end of file):

	```
	---
	ip: "192.168.10.10"
	memory: 2048
	cpus: 1
	provider: virtualbox

	authorize: ~/.ssh/id_rsa.pub

	keys:
	    - ~/.ssh/id_rsa

	folders:
	    - map: ~/Projects
	      to: /home/vagrant/Projects

	sites:
	    - map: homestead.app
	      to: /home/vagrant/Projects/laravel/public

	databases:
	    - homestead

	variables:
	    - key: APP_ENV
	      value: local

	# blackfire:
	#     - id: foo
	#       token: bar

	```

14. Edit hosts file /etc/hosts and add this line at its end:

		192.168.10.10 homestead.app

15. Setup secure shell (ssh) key

		ssh-keygen -t rsa -C "you@homestead"

16. Run homestead virtual machine

		homestead up

17. SSH to homestead virtual machine

		homestead ssh

18. Install Laravel in your Projects directory (/home/vagrant/Projects)

		composer create-project laravel/laravel --prefer-dist

19. Now open your browser on Mac and type [http://homestead.app](http://homestead.app)

You will get fancy welcome screen of Laravel 5.

![Laravel 5](/assets/laravel-homestead-the-real-installation-guide/laravel-5-welcome-1.png)

By the way, take a look inside Projects folder on your Mac. You will find laravel subfolder there.




If you want to use Lumen, just get out of vagrant virtual machine (exit ssh) and install Lumen using composer:

		composer global require "laravel/lumen-installer=~1.0"

SSH again into running Homestead virtual machine and create Lumen installation in Projects directory:

		composer create-project laravel/lumen --prefer-dist

To add Lumen site, just edit Homestead.yaml (homestead edit) and add new site into -sites: section of this configuration file.


```
---
ip: "192.168.10.10"
memory: 2048
cpus: 1
provider: virtualbox

authorize: ~/.ssh/id_rsa.pub

keys:
    - ~/.ssh/id_rsa

folders:
    - map: ~/Projects
      to: /home/vagrant/Projects

sites:
    - map: homestead.app
      to: /home/vagrant/Projects/laravel/public
    - map: lumen.app
      to: /home/vagrant/Projects/lumen/public

databases:
    - homestead

variables:
    - key: APP_ENV
      value: local

# blackfire:
#     - id: foo
#       token: bar

```

Once you have this, you need to add lumen.app to you /etc/hosts:

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost
192.168.10.10   homestead.app
192.168.10.10   lumen.app
```

Restart Homestead VM:

	homestead halt
	homestead up

I don't know why, but **homestead provision** does not seem to reflect changes in Homestead.yaml file.


Then you can access Lumen installation from Safari on http://lumen.app





Ok, now that you have it up and running, let's see what happened here.

### Explain

You can install a lot of development stuff right into your Mac and sometimes you might not even have another choice (like XCode), but sometimes you have. If you want to keep your Mac isolated from different development environments, you can use virtual machines. I already run Windows 7 and Ubuntu Linux on Parallels Desktop, so I can easily setup new Ubuntu right there and then install all tools for my development environment, but it's a lot of installing and editing of config files, etc. Wouldn't it be great if you can somehow get everything out of the box? I mean not only operating system, but all the tools coming with it already set up specifically for your needs. Well, now you can achieve this with Vagrant.

I don't want to go to details, because you can always go to [http://www.vagrantup.com](http://www.vagrantup.com) and learn everything about Vagrant. I will just say that this is a tool that will help you setup quickly and easily  portable development environments. You can go even further with Docker where you don't need full operating system as with Vagrant box, but that's another story.

I tried my best to but couldn't find the way to run Homestead inside nested virtual machine. I wanted to get even further and totally isolate everything from my Mac OS X (even Vagrant!) and run it all inside dedicated VM. But Vagrant is tied with VirtualBox and that's it's weakness because VirtualBox does not support nested VM properly. Host VM does not pass all the stuff to guest VM. Vagrant offers some paid solution with VMware instead of VirtualBox, but I didn't go that far.

This is as far as I got:


>Sometimes especially in nested VM (Ubuntu VM on Mac via Parallels Desktop in my case) *homestead up* will end up with *default: Warning: Connection timeout. Retrying...* repeated many times.

>It looks like when you run *homestead up*, it aborts nested VM during initialization process. When it starts VM again, VM needs you to select the mode in which it should run, but since you don't see this GUI screen, you have no idea what's going on and why you see only Connection timeout while Ubuntu inside VirtualBox is waiting patiently for you input (selection from start menu).

>To fix this just start Virtualbox GUI from virtualized Ubuntu terminal. You will be presented with *Oracle VM VirtualBox Manager* where you will see your Homestead VM with the state *Aborted*.

>Start Homestead VM from *Oracle VM VirtualBox Manager*, wait for welcome screen and select \*Ubuntu. Wait some more for few error messages(tsc, soft lockup) and Ubuntu 14.10 loading screen with another error. Then it should just run with no other error messages and in you will see *Running* state in VM Manager. You should be able now to ssh into homestead from new terminal window.

After more testing of this workaround I realized it doesn't work every time and that the main problem is in VirtualBox and it's bad support for nested virtual machines.


On the other hand it's not really wise to run VM inside VM, because it will consume a lot of resources. Imagine you have:
1) hypervisor (e.g. VMware Fusion) running on Mac
2) Linux running inside that hypervisor
3) another hypervisor (VirtualBox) running on virtualized Linux
4) Homestead (which is basically another Ubuntu Linux setup for Laravel development)

So in order to run Laravel you need A LOT of resources.

With Vagrant running directly from Mac OS X, you save step 1 and 2 and run only VirtualBox and Ubuntu (Homestead) inside as VM.

There is of course another way. Drop Homestead and install Laravel manually (not that hard at all). I tried this with Ubuntu on VMware Fusion. But it is somehow slower especially when you use PhpStorm. So I gave up with experiments and settled with the solution described above.

If you followed my steps you have a brand new virtual machine installed, configured and running in VirtualBox. This virtual machine is ready for development, because it contains PHP, HHVM, Nginx, MySQL, Postgres, Node (With Bower, Grunt, and Gulp), Redis, Memcached, Beanstalkd, Laravel Envoy, Fabric and HipChat Extension. I don't even know what all this means, but I know I have it ready so I don't need to configure it myself, which is a huge time saver (EDIT: Not really, compared to setup of nginx with php on Ubuntu, you won't save so much time, but it's neat.)

So you have your Mac running OS X. You have virtual machine running in VirtualBox called Homestead. This is basically just regular Ubuntu Linux configured by folks behind Laravel, so you don't need to install all the required stuff manually.
