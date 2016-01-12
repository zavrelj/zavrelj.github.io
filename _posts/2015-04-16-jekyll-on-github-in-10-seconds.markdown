---
layout: post
title:  "Jekyll on GitHub in 10 seconds"
date:   2015-04-16 22:21:20
tags: github jekyll
---

Today I will show you very straightforward way to setup [Jekyll](http://jekyllrb.com) on [GitHub Pages](https://pages.github.com). It's so easy I wonder why something like this is not available on Jekyll site.

Prerequisities to save a lot of time:

1. Login to your GitHub account or create one.
2. Download and install GitHub client for Mac.
3. If you don't already have Xcode, download and install Xcode Command-Line Tools, [here is how](http://railsapps.github.io/xcode-command-line-tools.html).
4. Whenever I use **zavrelj** in this guide, use your own GitHub account name instead.

Ready? Fine. Let's get to it:

1. Fire up your browser and go to your GitHub account.
2. Create new repository
3. Name it **zavrelj**.github.io
4. Hit "Create repository" button

 ![](/assets/jekyll-on-github-in-10-seconds/git-1-2.png)

5. On new page hit "Set up in Desktop" button

 ![](/assets/jekyll-on-github-in-10-seconds/git-2-2.png)

6. GitHub for Mac will automatically start now asking you where to clone your repository. Locate ~/GitHub and hit "Clone" button.

7. Fire up you terminal and type:

        sudo gem install jekyll

        cd ~/GitHub/zavrelj.github.io

        jekyll new .

8. Back to GitHub for Mac client where you have already new files waiting for commitment.       ![](/assets/jekyll-on-github-in-10-seconds/git-4.png)

9. Write some description and hit "Commit to master" button.

10. Hit "Publish/Sync" button

11. Back to your browser and go to http://zavrelj.github.io

12. Welcome to Jekyll!

 ![](/assets/jekyll-on-github-in-10-seconds/git-5.png)
