---
layout: post
title:  "Hexo on Netlify"
date:   2015-05-12 22:21:20
tags: hexo netlify
---

I want to test [Netlify](http://www.netlify.com) with [Hexo](http://www.hexo.io). And I want to use [Docker](http://www.docker.com) and [Vagrant](http://www.vagrantup.com) to separate Hexo environment from my Mac. The idea is to generate content via Hexo running as Docker container inside Ubuntu Snappy in Vagrant. Once the content is generated I will upload it to Netlify server. Pretty complicated, but we will learn a lot along the way.


Let's start with Vagrant setup. I have Vagrant already installed, if you don't, it's pretty straightforward. Just install [VirtualBox](http://www.virtualbox.org) and [Vagrant](http://www.vagrantup.com).

To setup new project I will create it's own directory and cd into it:

    mkdir ~/Vagrant/hexo_snappy
    cd ~/Vagrant/hexo_snappy

Now I will initialize this directory for usage with Snappy:

    vagrant init http://cloud-images.ubuntu.com/snappy/15.04/core/stable/current/core-stable-amd64-vagrant.box

And run it with:

    vagrant up

This will download Snappy distro from Ubuntu server.

Now you can easily login into Snappy:

    vagrant ssh


___

In next step I will install Docker to Snappy. This is really easy. Just type this command:

    sudo snappy install docker

Now we can download and run Hexo container from Docker image.



No port forwarding in Ubuntu Core yet?!



On Mac with Kitematic






To start I will create slightly modified version of  [simplyintricate/hexo](https://registry.hub.docker.com/u/simplyintricate/hexo/) container in order to get access to \_config.yml file.

Let's prepare new hexo folder with new Dockerfile first:


    cd ~/Kitematic
    mkdir hexo
    cd hexo
    touch Dockerfile



I will now download and run simplyintricate/hexo container via Kitematic in order to extract original \_config.yml from it to my own hexo folder like this:

    docker cp container-name:/usr/share/nginx/html/_config.yml ~/Kitematic/hexo

___

Now I will modify \_config.yml to meet my needs, mainly change the site name and the author as well. I need to replace default \_config.yml inside container image with my own, so I add instruction to Dockerfile:


    FROM simplyintricate/hexo
    MAINTAINER Jan Zavrel <jan@zavrel.net>

    ADD _config.yml /usr/share/nginx/html/_config.yml

___

and build my new image from Kitematic's terminal (small whale icon):

    cd ~/Kitematic/hexo
    docker build -t zavrelj/hexo .

Now I can run my container:

    docker run -t -i zavrelj/hexo /bin/bash



It won't display anything in browser for the first time, because:

1. Ports are not mapped.

I don't know why, but I have a workaround.

Just stop the container and restart it from Kitematic UI.

2. Volumes are not enabled, so there is no folder structure yet, because container name is randomly generated.

Just click on **usr** folder of running hexo container inside Kitematic. This will create folder structure inside ~/Kitematic folder under container name.

Then Hexo will be hapilly running on my local address presenting basic initial page.


Once I have my initial setup done, I can move to creating some content.

**To do so, just run hexo generate aaaand... there's an error....**
