---
layout: post
title: Vagrant, we meet again
comments: true
---

I haven't logged into my desktop vagrant box since October 2015. After logging in a couple of days ago, I managed to mess things up, and lost my VM (might have to do with the VirtualBox upgrade). It wasn't a big deal though, it was the box spun up from the [Getting Started](https://www.vagrantup.com/docs/getting-started/) tutorial. Although, I did attempt to fix the problem by following [this blog post](http://zachwills.net/fix-for-vagrant-not-detecting-existing-vm/) but ultimately failed as when I did a `vagrant up` it would not connect because of authentication and so I disposed of the box. In the process of trying to acquire a new Linux environment, I found out that 64-bit boxes would not work for whatever reason (this needed some looking into that I clearly didn't want to do, vagrant bugs are notorious and I didn't want to spend hours trying to fix this, speaking from experience), The boxes that I attempted to install are **hashicorp/precise64** and **ubuntu/trusty64**, both of which failed. And so, I had to stick with my old **hashicorp/precise32** box.

The reason that I needed a Linux environment in my Windows machine was to run some C++ programs. I am currently a TA for an introductory course that requires me to grade programs. I didn't want to do things on my small laptop screen and thought things would be much smoother on my larger 23-inch desktop monitor. Yeah, I know that it is completely overkill as all I needed to do was install [MinGW](http://www.mingw.org/) and the compiler. But after getting used to working on Linux on my laptop, MinGW is just not good enough. Who knows, maybe I might need to run some Python later on, or anything that is a nightmare setting up on Windows (everything). 

After loading the box, I did a quick `vagrant ssh` and ran the usual:

```shell
sudo apt-get update
sudo apt-get upgrade
```

to get the machine up to date, and proceeded to install the software I needed, `gcc, vim, git and some python tools`.

Process was relatively fast, about 10 minutes and I now had a fully working Linux environment. Maybe that is a bit of a stretch as I can only access the box through `ssh` but good enough. With some basic tooling up, I needed to re-create my development environment which mainly includes of adding my `.vimrc and .bash_aliases`. I have had to do this process a couple of times from the servers that I have had to configure so it might be time to learn some shell scripting and automate this process.

## Conclusion
If you need a quick and dirty way to set up a Linux environment on Windows, the **Cygwin + Vagrant** combo is pretty good. The only downside is that you to learn some UNIX basics, and it's preferred if you know Vim, although they do provide you with a shared folder where you can edit files with your native editor.


### References
* [My dotfiles](https://github.com/danielcodes/dotfiles)
* [Vagrant boxes](https://atlas.hashicorp.com/boxes/search)
* [Unix basics](http://www.ee.surrey.ac.uk/Teaching/Unix/)

