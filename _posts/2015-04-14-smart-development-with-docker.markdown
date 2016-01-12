---
layout: post
title:  "Smart development with Docker"
date:   2015-04-14 22:21:20
tags: docker
---

My first steps in development go back to 1994 when I met PC for the first time in high school. DOS and Pascal, local directories of code. Later, when I started to work for German company Caatoosee A.G. it wasn't really better in the sense of version control or multiple development environments. These days it's different.

Version control can be done using free tools, so there's no excuse now not to use it. Setting up different development environments is also quite easy, especially via virtualization. And recently it also became quite cheap to have your own VPS with dedicated IP address thanks to [DigitalOcean]() services.

I will briefly describe what was it like for me, when I developed my DS project (you can imagine it as some old old predecessor of Facebook) and what it would look like to develop it today using all the tools available.

My project was based on usual suspects - PHP, MySQL and Apache. At that time I had PC with Windows as development machine and posted final (production) version to shared webhosting running on Linux. From time to time I had to solve different settings, but in the end I was able to sort it all out with help of webhosting provider tech support.

I remember how sad and disappointed I was when I finished some new features on my local machine, only to find out that it didn't work properly, when I uploaded it to webhosting server. Sometimes I wasn't even able to implement everything I wanted to, because at that time a lot of stuff accessible these days via CPanel simply was not available, like cron service.
I learned to live with limited non-root access, but felt that it was far from ideal. I was not able to have the same version of PHP sometimes, not that I needed some special new features, but the idea, that I had different environment on my local machine and webhosting server was not very comforting. When I started to look at some RAD environments like CakePHP of CodeIgniter, I was simply out of luck. My provider didn't allow me to install required stuff (no root access) and didn't want to break his server by installing it for me. This forced me to create everything from scratch.

I knew there was a solution. Virtual private server. But it was out of my financial realm, after all this was only a study project for a few of my friends. I didn't have any intention to monetize it and I did not even know how.

So, that was it. Local machine cluttered with PHP, MySQL and Apache packed in WAMP. Shared webhosting configured for simple usage. No root access, no way to use any RAD.

And today? There's no today for my DS Project. It is safely stored, not really touched since 2007. But I got the idea to revive it. Not to create anything out of it, just to try to employ all the tools available today including VPS and see what it would be like to work on it again.

*1. Server*

I know for sure I don't want to use shared hosting. It's completely different world today with CPanel, you can easily setup subdomains, many databases, edit Apache and PHP settings, but when you need for any reason to restart httpd, you are again usually out of luck. Not with your own server. As I mentioned above I use [DigitalOcean]() cloud service aimed for developers. For a few bucks per month you get your own machine (sort of) and you can do whatever you want with it. But DigitalOcean (DO) goes even further with Droplets (more about it in another article) so you can easily create complete development environments specifically for your needs if you feel you need it.

*2. Development environment*

I have Mac and don't intend to install PHP, MySQL and Apache on it. I simply do not want to mess it up and with virtualization I don't even need to.
I see there are three layers of virtualization today:

1) You can grab VirtualBox, download Ubuntu image, install your very own virtual Ubuntu operating system and then set it up that way you need for developing PHP web apps.

2) You can go even further. Use Vagrant, download specific box, which is operating system (typically Ubuntu) already setup to develop in PHP.

3) You can use Docker. I believe this is the best approach and sometime I will write my reasoning behind this opinion.

*3. Version control*

This is something I never used. My version control was all about commenting out deprecated code so I could use it later if I needed it. Today I will go with Bitbucket.


**Let's start!**

OK, so I have a Mac with iTerm2 and I want to setup my development environment, do some work on my web application and that package it and send it directly to my VPS. I want to make sure everything works identically so everything needs to be setup identically.

I will use Docker to get the same environment on my Mac as well as on my VPS.

**How to get Docker on Mac?**

Grab [Kitematic]() and run it. It will start Docker VM (Linux virtual machine) and present you with nice pannel where you can create new container from recommended images.
![Kitematic](/assets/smart-development-with-docker/Kitematic.png)

I want to work on my DS project so I just need something like Linux-Apache-PHP-Mysql combo to be able to run my project. It is possible to get just that by searching for *lamp* and choosing the one you like most. I would go with tutum/lamp because it has 71 stars - quality indicator I guess, but comments below on its repository page [](https://registry.hub.docker.com/u/tutum/lamp/) show it does not work very well, so I just move on. Next is reinblau/lamp. Looks good, so we will download it.
![Reinblau Lamp](/assets/smart-development-with-docker/reinblau-lamp.png)

Once downloaded it will be started automatically and you can test your new development environment in your browser. My local IP was setup to 192.168.99.100 with port 49153 and it works, webserver is running.

**This part is quite important to understand the concept, so bear with me:**

Kitematic for Mac uses special way to run Docker containers. Docker client runs on OS X, but Docker server runs inside it's own virtual machine. You need different terminals to communicate with them.

**Why is it so dificult?**

Well, Mac OS X is not Linux, Docker needs Linux, so you give it Linux. Docker on Mac is in fact running on lightweight linux distribution called **boot2docker**). It's all hidden inside Kitematic, but if you start Kitematic, you will see the screen with "Starting Docker VM". That's your Linux virtual machine starting.

![](/assets/smart-development-with-docker/docker-vm-1.png)

You cannot access Docker client from your OS X terminal. Try it. Fire up your Mac OS X terminal and try something like this:

		docker images

You will probably only get this error:

		FATA[0000] Get http:///var/run/docker.sock/v1.17/images/json: dial unix /var/run/docker.sock: no such file or directory. Are you trying to connect to a TLS-enabled daemon without TLS?

To be able to communicate with your Docker client via command-line, you need to open terminal which is Docker CLI (Command Line Interface) ready. Luckily Kitematic has it all setup for us. To run Docker CLI terminal, you just need to click on that small whale icon at the left bottom of Kitematic window.

![](/assets/smart-development-with-docker/Docker-client.png)

Once you are in Docker CLI terminal window, you can run

		docker images

and get the list you wanted.

To stop the container, you just type it's flag name (without repository). In our case that would be:

		docker stop lamp

Now you can see in Kitematic window that lamp is greyed out.

To start it again, go to the Docker CLI terminal and type:

		docker start lamp

**How to get inside container?**

Now that we know how to comunicate with docker client, let's move to Docker server. Normally, to get inside container you would need to ssh into **boot2docker** and use **nsenter** program. With Kitematic, it's way easier. Just use built in terminal by clicking on the third upper icon, just like in picture below.

![](/assets/smart-development-with-docker/Docker-server.png)



Ok, now we know that we have sort of three terminals:

1. Mac OS X terminal - not usable
2. Docker CLI terminal - to use docker client commands
3. Docker server terminal - to get inside container


**Back to our lamp container**

Since there's no volume mapping www-data folder, I would like to modify this container.

It's quite easy. I can use reinblau/lamp container and add only what I need.

To do that, I will have to create Dockerfile. Kitematic stores it's images inside ~/Kitematic, so I need to get to that directory from docker CLI terminal first, create new directory for my new Docker image and then create blank Dockerfile inside that directory:

	  cd Kitematic
    mkdir zavrelamp
    cd zavrelamp
    touch Dockerfile

Once Dockerfile is created, I can easily open it with Textedit and type these lines:

		FROM reinblau/lamp
    MAINTAINER Jan Zavrel <zavrelj@gmail.com>

    VOLUME /var/www/

Now I just save the changes, go back to CLI terminal and build the image:

		docker build -t zavrelj/zavrelamp:v1 .

where v1 is the tag of my image and the dot in the end specifies where Dockerfile is located (in my case in current directory, hence the dot).

Once the image is built, I can create new container from that image:

		docker run -t -i zavrelj/zavrelamp:v1 /bin/bash

This will add new container in my Kitematic window and get me inside container in terminal.

Now if I check my Volumes in Settings, I can see that I have a new folder created:

![](/assets/smart-development-with-docker/new-folder.png)


Since this was success, I might want to keep this image, to be able to create container from it later. This is why I will upload my new image to my Docker.com repository.

But before I do that, I will change the tag to *latest* which is the default tag, so I don't have to remember which version I have in my repository.

To do that I will just run:

		docker tag 5db5f8471261 zavrelj/zavrelamp:latest

where 5db5..... is image id you get from the list of images when you run

		docker images

and you don't even have to type it all, first few letters is enough.

Well, we have our new tag, so we can now upload image to repository.

To do that, I will just type this command to CLI terminal (don't forget to exit from inside container if you're still there):

		docker push zavrelj/zavrelamp:latest

You don't even have to type *:latest* at the end, since it's the default value.

Once the image is uploaded, I can remove container and image from my Kitematic.

		docker rm hungry_jung
		docker rmi zavrelj/zavrelamp:latest

Now whenever I want I can pull my image from my repository (by default it will pull *latest* version):

		docker pull zavrelj/zavrelamp

and create container again:

		docker run -t -i zavrelj/zavrelamp /bin/bash

This will not only create container, but run it as well.

To stop the container, access it by randomly generated name which is displayed in Kitematic over the name of image (in my case hungry_sammet):

		docker stop hungry_sammet

And to start it again, you guessed it:

		docker start hungry_sammet

Ok, we have our container up and running. Next step is to assign specific folders from our Mac to directories inside container. To do it manualy could give us some headache, but with Kitematic, it's again piece of cake.

Go to Kitematic window, where you should now have three directories. Remember how we used reinblau image and add new volume to our Dockerfile? Well, this is the result. To assign these directories to Mac folders, you can manually select them in Settings -> Volumes or just double click on folder icons.

![](/assets/smart-development-with-docker/mapping-folders.png)

I'm quite happy with default folders for those container directories which are created inside Kitematics folder of my container. This is how it will look like:

![](/assets/smart-development-with-docker/mapping-folders-paths.png)

The point is, that we can now easily put our code into Mac directory called **~/Kitematic/name-of-container/var/www** which is easily accessible via Finder, and it will be automatically used as Apache web directory.

Let's try it! Create simple info.php file, which will display php settings (and check, that PHP is installed and running in our container).

Just create blank file named phpinfo.php in www directory mentioned above and write this inside:


	<?php

	// Show all information, defaults to INFO_ALL
	phpinfo();

	?>

Easy, huh?

Now run your browser and point it to IP address defined in Kitematic's Settings -> Ports. In my case it would be
http://192.168.99.100:49153/phpinfo.php

If everything went well, you will see this in your browser:

![](/assets/smart-development-with-docker/phpinfo.png)

If not, you can always pull image from my repository:

		docker pull zavrelj/zavrelamp


This article is work in progress, so expect more, but I wanted to make it available to anyone currently stucked with Docker on Mac, so here it is.
