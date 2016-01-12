---
layout: post
title:  "How to setup SSH key"
date:   2015-04-05 22:21:20
tags: linux
---

Password is so old school. It's not too hard to guess it, you have to remember it or write it down somewhere. If you need to connect your computer with another computer (server), there is a better and safer way.

You can use secure shell or SSH and in order to tell server, that it is in fact you who is trying to talk to it, you will give it your public key. Then, when you connect to server, public key is paired with private key which is safely stored on your computer.

In this article I will show you how to setup new SSH key in order to later use it with GitHub, DigitalOcean Droplets and many other services.

You might already have ssh key generated on your machine and not know about it. If you followed some tutorials, for example my own [Laravel/Homestead installation guide](), it is possible you still have your own ssh key already generated. To find out if this is the case is quite easy.

Open Terminal end enter:

        ls -al ~/.ssh

If you already have ssh key generated, you will get a list of files (at least **id\_rsa** and **id\_rsa.pub**).

If you get error message, that ~/.ssh does not exist, you didn't create any ssh key yet, which is exactly what we're about to change now.

To generate public/private pair of rsa key, simply enter this line to Terminal and hit **ENTER**:

		ssh-keygen -t rsa -C "your_email@example.com"

You should use your own e-mail of course, like smith@gmail.com. Leave Terminal to save your ssh key to default location and simply hit **ENTER** when asked.

Then you will be asked for passphrase. I strongly suggest you type it, even though it's possible to leave it empty. Strong passphrase will protect your ssh key. Think about it as another layer of security. Anyone who gain access to your computer has automatically access to all services that are set up to connect via SSH if passphrase is not set.

You should store your passphrase securely. I suggest 1Password for example. If you're not sure later what your passphrase is, you can easily test it by typing

       ssh-keygen -y

Terminal will ask you where the file with the key is (by default you just hit ENTER), then type your passphrase. If it's right, you'll get your key, otherwise you will get **load failed** error message.

If you see fingerprint of your ssh key on Terminal now, you have sucessfully created public/private rsa key pair. Now it's good idea to add it to your Mac Keychain, so you don't have to enter it every time you need it.

To that ensure that ssh-agent is enabled (should be on Mac since Leopard):

		eval "$(ssh-agent -s)"

and then enter this:

		ssh-add ~/.ssh/id_rsa

Now you have your SSH key safely stored in Keychain. You can use it to connect to your service of choice.

To get your SSH key to your clipboard in order to paste to specific form, just type:

     pbcopy < ~/.ssh/id_rsa.pub
