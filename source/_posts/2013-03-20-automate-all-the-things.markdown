---
layout: post
title: "Automate all the things"
date: 2013-03-20 11:30
comments: true
published: true
categories:
- tech
- devops
- puppet
- vagrant
- osx
---

Disaster Strikes
================

Last week my Mac book ran out of space again, this had been going on for some time and I'd had enough.
I was spending an ever increasing amount of much time hunting down rogue space invaders on my small 120gb hard drive.  So **something** had to be done.

<p class="center">
	<strong>In times like this there is only one solution!</strong><br>
	<img src="/images/2013/mrragequit.jpg" title="Original image: http://leviathyn.com/games/news/2012/06/14/your-worst-rage-quit-moment/" /> <br>
	<small> Source: <a href="http://leviathyn.com/games/news/2012/06/14/your-worst-rage-quit-moment/"/>http://leviathyn.com/games/news/2012/06/14/your-worst-rage-quit-moment/</a></small>
</p>


When upgrading to Mountain Lion I decided to start afresh and reformat my hard drive.
Normally starting from scratch would be a royal pain in the backside, taking many days to get things installed and allow me to get back to coding.
Then I remembered hearing about [boxen](http://boxen.github.com) which could give me an easy way to rebuild and *maintain* the software on my Mac and hatched a plan of devops style automation!

My automation trifecta
=======================

<!--more-->

Boxen
-----

Boxen is built on top of [puppet](https://puppetlabs.com/) and it allows you to describe in code your Mac configuration and what software to install on it. To get started just copy githubs' starter [boxen repo](https://github.com/boxen/our-boxen) and then add your requirements.
I had to do two things to get my software stack automated:

  1. In the main puppetfile link to the github sources and versions
  2. In my personal puppet manifest (modules/people/manifests/rozza.pp) include the classes for the software I want installed.
  3. Profit?!

Check it into github and run boxen. Volia a few minutes later my initial stack comprising; *Adium, Chrome, Dropbox, Intellij, Iterm2, Skype, Slate, Spotify, Sublime Text 2, Vagrant, Virtualbox, wget and Zsh* were all installed and ready to use.
No wasting time hunting down .dmgs and navigating through their respective websites to download them.
If you want to know more about boxen then check out this [presentation](https://speakerdeck.com/wfarr/boxen).
All my configuration is now on github [my-boxen](https://github.com/rozza/my-boxen/)

dotfiles
--------

Boxen is great as it automates the software installed on my mac but what about my general configuration? I could use Boxen and puppet to define and create *all* my configuration files - but that seems unnecessarily complex and to me there is a separation of concerns. So when I asked on twitter what other folk do [Brad Wright](http://intranation.com) pointed me to his dotfiles.

After a quick investigation I duly <span style="text-decoration:line-through;">raided and plundered</span> took inspiration from and now I have all my personal dot file configurations for things like *github, slate and zsh* backed up on github.  What about private configuration files? Where it makes sense any files with a <code>.local_</code> prefix will be sourced by their respective core configuration file.  That way I can have different configrations across machines - I can have a core <code>.bashrc</code> and a local version on each machine eg <code>.local_bashrc</code>.  All my public [dotfiles](https://github.com/rozza/dotfiles) are on github so plunder away!

Vagrant
-------

The final piece in my automation puzzle is Vagrant.
I've been a huge fan of using vm's for development for years and more recently [vagrant](http://www.vagrantup.com/) has been my tool of choice.
Installing things with apt-get on a linux vm just saves me time and pre-[10gen](http://10gen.com) my vms used to mirror my production environment.
As its a vm all I had to do is back it up before I wiped the hard drive and I could use that to get up and running
(I know its vagrant and had I been more diligent all I would have needed was the vagrant file and base box).

When working on a vm, I still like to use sublime text on my mac to code.  So the way to achieve this is to nfs the code directory from the Mac into the vm - I used to do it the other way round, but having the code local to the mac makes editing very snappy.  The last thing to do was back up my code directory - I was surprised that all my checked out code weighs in at a hefty 13 gigs!

Automation heaven
=================

<p class="center">
	<img src="/images/2013/make-a-boxen.png" /><br>
	<small> Source: <a href="https://speakerdeck.com/wfarr/boxen"/>https://speakerdeck.com/wfarr/boxen</a></small>
</p>

By using a combination of boxen, vagrant and dotfiles I massively reduced the time needed to rebuild my mac and get productive.
By backing up 13gb of source code, it meant I managed to reformat, install Mountain Lion and configure my Mac and get coding again in under 3 hours!
That included the time it took me to learn how to use boxen and setup dotfiles!

Of course I will tweak those repositories overtime but all my core configuration for my Mac is now in a git repo on github [my-boxen](https://github.com/rozza/my-boxen/) for the software and [dotfiles](https://github.com/rozza/dotfiles) for configuration.

I'm loving this adventure and if want to backup your software configuration or you're getting a new Mac, then jump on board and automate all the things&trade;.

**PS**. Feel free to clone / copy my repo's all I ask is if you add something cool you either comment below or [tweet me](http://twitter.com/RossC0) so I can use it to should I need to!


