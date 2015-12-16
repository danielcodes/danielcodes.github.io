---
layout: post
title: On using Tmux 
---

I've been wanting to write this one for a while now. This summer, I got learn two very exciting tools, **Vim** an **Tmux**. These two are a must-have for any terminal junkie and together they've taken my workflow to a whole new level. Although tmux has some extended functionality, in this post I am going to focus on how I personally use it. Also, I am going to assume that you've installed the tool on your machine and know how to fire it up. Ok, onto the big question, what do you customize?

###Changing the prefix

The first thing you ought to do is change the pesky default prefix mapping, **CTRL+b**. To do this, first create your **~/.tmux.conf** file, this file allows you to configure tmux settings to your liking. In there, add: 

```sh
#prefix to ctrl+a
unbind C-b
set -g prefix C-a 
```

However, your pinky finger still has to travel a mile to get to that CTRL key at the bottom left. To fix this, I'd recommend looking into your system and changing the **Caps Lock** key mapping to **CTRL**. I have Ubuntu-Gnome, and it came with a nice little piece of software called Tweak Tool, here I was able to turn my Caps Lock key into CTRL.

###Refreshing the .tmux.conf file
Next, you gotta know how to refresh these updates that you're adding. Usually, it is done by CTRL+a followed by :, then on the prompt you type, 

```:source-file ~/.tmux.conf```

but this is cumbersome and hard to remember, instead add:

```sh
#reload of the config file
unbind r
bind r source-file ~/.tmux.conf
```

this will allow you to refresh with CTRL+a + r.

You can test this by changing window tab colors on the status bar, add,

```sh
#highlight current window
set-window-option -g window-status-current-bg white 
```

try out different colors and refresh with CTRL+a + r.

###Pane Splitting

Now that we have much nicer prefix **CTRL+a**, what comes next is pane splitting. The defaults are set to " (vertical) and % (horizontal). These bindings are hard to memorize and not so intuitive, let's change them. In your *.tmux.conf* add:

```sh
#intuitive pane splitting
bind | split-window -h -c "#{pane_current_path}"
bind - split-window -v -c "#{pane_current_path}"
unbind '"'
unbind %
```

The first line is saying to use the **|** key to do a horizontal split. The ```-c "#{pane_current_path}"``` part makes it so that the split pane remains in the current path where you split from. Imagine being into directory that's 10 levels down, and having to cd there again when you create a new pane. You get the picture.

###Moving between panes
By now, you can split panes, but how do you move aronud? One way to go about it, is to hit the prefix key followed by an arrow key movement. This is sloppy though, as you have to move your hand down to the arrow keys. A better way is the following, add:

```sh
#quick pane cycling, prefix + Ctrl-a 
unbind ^A
bind ^A select-pane -t :.+
```

with this, you can move from pane to pane by hitting your prefix twice.
If you mapped your control key to CAPS LOCK, you can hit the prefix once, keep your pinky on CAPS and press *a* again. Ah, so, so, efficient.

PS. I found this tip [here](https://robots.thoughtbot.com/a-tmux-crash-course)

If by any chance, a pane crashes on you, you can close it with **prefix** + x.

###Zooming in
One final feature that I want to talk about is zoom. Having multiple panes is great and all but truth is most of the time your focus is only one, the pane in which you're writing code. Tmux has a neat little feature that helps with this problem, do **CTRL+a + z**. This command, full screens the current pane, to exit, just use the same command.

TL;DL My .tmux.conf is [here](https://gist.github.com/danielcodes/ea6ee30d2ff032421b2e). Leave me a comment if I can improve on anything.

Hope you found these helpful, and happy tmuxing !

