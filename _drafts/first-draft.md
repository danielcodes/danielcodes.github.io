---
layout: post
title: Building a Simon game
comments: false
---

Outline
-understading the project
1st TODO
-make a div blink by clicking it
one article was the most helpful and it did the thing I wanted, link 2 articles
but it is still a hack, talk about delay and animation duration
bug using a reg fun in map, talk about the key key property

2nd TODO
blinking div works
now animate a sequence of them, really didn't know where to start here, talk about first idea of a for loop?
read up a bit on react motion, it seem to solve a bigger problem
but I read an article up on it anyway, here I found the answer to my problem
=> to animate the sequence, i had to sequentially play the blinks, done with a setTimeout
bug with dissapearing and reappearing blocks, if processor is too slow
-seq that plays, can go up a level and resets on bad input

3rd TODO
I can play a sequence, and up go up levels
need a timer for when a user plays, referenced an old project and looked at documentation for timeouts
=> idea and implementation of timer, maybe even show a pen?
disabled clicks when they're not asked for, lotta bugs
hard reset function, explain how this is done

4th TODO
adding the sound
-read up a bit on how to apply ajax to react apps, planned on using axios until the request didn't go through
-turns out that I can load the audio directly from the source from the audio API

Logic done.

Fixing the UI
-started by creating the 4 quarters, link it up
-created the controls section, started by boxing everything, and did a lot of centering and customizing heights and widths, a lot of boring and repetitive work
-at this point, I migrated things over to the React app, noticed a bug with spacing
-attempted to add a toggle switch, spent a good while thinking that if I loaded a JS file, I'd have access to the component, boy was I wrong, and when i attempted to copy and paste from a blog post, React had a problem with the input tag, oh well
-settled for the same mechanism as the strict button, button that lights up a light

5th TODO
adding functionality to the ON/OFF toggle button
-on is nothing new, off is where things get interesting
-this is due to me developing the UI with my screen size set at 75%, this is to see more code
-had some people try it
-final touches where giving the buttons the press feel, adding more time for higher level and add a msg for winning

That's a wrap.



