---
layout: post
title: GUI setup madness
comments: true
---

This weekend I spent a solid day trying to configure my computer to run FLTK with the examples from this book [here](http://stroustrup.com/Programming/). FLTK is software used to write graphical applications. So far my experiences with these types of set ups have been nothing but disastrous. A semester ago, I had to install Qt, the process was pretty nightmarish, there wasn't a proper tutorial and it was basically me searching all of stackoverflow. In the end I got it, but the experience was quite painful and something that I thought I wouldn't have to go through again. Fast forward, here I am, in need to install GUI software again. Can you feel what's coming?

### Lesson not learned

From my experience with Qt, I should have known that the process was going to be a bit tricky. But even so, I ignored everything and thought that a simple google search would solve things (Oh, how wrong was I). After a bit of searching I found that I needed to get the tarball (later found that there was an apt-get package), unzip the tarball and run some commands. It went something like this...

First `tarball`, which was done with much help from [here](http://askubuntu.com/questions/25347/what-command-do-i-need-to-unzip-extract-a-tar-gz-file),

```sh
tar -xvf file-ending-with.tar.gz

# the flags
# x extracts, v outputs files being extracted, f for path and name of file
```

This was followed by me running some commands, to install the software,

```sh
# I got these instructions from somewhere in the internet

./configure
sudo make
sudo make install
```

From here on I gained access to the command line tool which was `fltk-config`, and to compile a file all that was needed was the `--compile` flag. Some samples worked, and some didn't. The one that needed to work, did not.

Later, I found that the `tarball` that I had unzipped, contained a `README` for Unix systems, describing the exact same process above. Man, I could have saved myself some time if I had been more attentive. Documentation is king.

In there, I was able to run a demo that worked fine. But most importantly, it told me where it had installed the files. They were located in the following paths,

* `/usr/local/include` for headers
* `/usr/local/lib` for libraries (files with ending with `.a`)

### The problem

I found out that the sample program wasn't compiling because of a bunch of missing headers. In the same link above, there is a link called `The complete collection of code ...`, another zip. Inside the zip, there was a `GUI` folder with the missing headers. I proceeded to place this `GUI` folder in `/usr/local/include`. This did not work, as the files where inside a directory (GUI/), and so I had to bring them out to the top directory, cluttering everything. After this, the program no longer gave the header errors but now there were undefined references everywhere.

### Theory

My gut tells me that the reason for the errors was a missing library (oh, I had to run `make` on the GUI folder to obtain a `.a` file). I couldn't solve this the way I had done above with the missing headers.

Check this,

![fltk](/public/img/fltk.png)

As you can see in those very tiny letters, the `fltk-config --compile file.cpp` command is an abbreviation for a `g++` compilation command which is looking for a very specific `libfltk.a` file. What I attempted to do was, place the missing library path there. And it still didn't work. And such was the end of my day. Filled with frustration and a lesson learned. 

### What not to do in the future

* Don't install software based on stackoverflow instructions, take the time to find the proper documentation (README inside zip file)
* Patience is a virtue, when things don't work, I start looking for the first hacky solution. Take the time to analyze the error

PS. I am most likely just going to install the software on my Windows Desktop as there is a proper instruction for how to do it there. 

