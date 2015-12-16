---
layout: post
title: On using Tmux 
---

I've been wanting to write this one for a while now. This summer, I got learn two very exciting tools, **Vim** an **Tmux**. These two are a must-have for any terminal junkie and together they've taken my workflow to a whole new level. Although tmux has some extended functionality, in this post I am going to focus on how I use it in my everyday workflow. 
Also, I am going to assume that you've installed the tool on your machine and know how to fire it up. If not, there are plenty of those on the internet. Ok, the big question, what do you customize?

###Changing the prefix

The first thing you ought to do is change the pesky prefix mapping, **CTRL+b**. To do this, first create your **~/.tmux.conf** file, this file allows you to configure tmux settings to your liking. In there, add: 

```sh
#prefix to ctrl+a
unbind C-b
set -g prefix C-a 
```

However, your pinky finger still has to travel a mile to get to that CTRL key at the bottom left. To fix this, I'd recommend looking into your system and changing the **Caps Lock** key mapping to **CTRL**. I have Ubuntu-Gnome, and it came with a nice little piece of software called Tweak Tool, here I was able to turn my Caps Lock key into CTRL.

###Refreshing the .tmux.conf file
TODO

###Pane Splitting

Now that we have much nicer prefix **CTRL+a**, what comes next is pane splitting. The defaults are set to " (vertical) and % (horizontal). These bindings are hard to memorize and not so intuitive, let's change them. To your *.tmux.conf* add:

```sh
#intuitive pane splitting
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
unbind '"'
unbind %
```

Wow, that's a lot of code. Ok, let's break it down. The first line is saying to use the **|** key to do a horizontal split. The *-c "#{pane_current_path}"* part makes it so that the split panes remain in the current path where you split from. Image being into directory that's 10 levels down, and having to cd there again when you create a new pane. You get the picture.

###Moving between panes
TODO








##what do I want to cover
-ca mapping
-splitting with | - and making it split current path
-refresh
-using ca ca to move around the panes
-using ca z to zoom into a tmux pane


