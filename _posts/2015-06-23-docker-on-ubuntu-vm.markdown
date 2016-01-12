---
layout: post
title: Docker on Ubuntu virtual machine
date: '2015-06-23 06:17:27'
tags: docker
disqus: true
---

To install Docker on Ubuntu you need slightly more than you'll get from official installation guide.

I tried to put it all together.

1. Get wget if you don't have it already

		sudo apt-get update
		sudo apt-get install wget

2. Install Docker

		wget -qO- https://get.docker.com/ | sh

3. Finish setup

		sudo apt-get -y install lxc
		sudo gpasswd -a ${USER} docker
		newgrp docker
		sudo service docker restart

4. Optionally install these two dependencies if the above does not work

		sudo apt-get -y install apparmor cgroup-lite
		sudo service docker restart
