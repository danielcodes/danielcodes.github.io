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


==============================================================

After finishing the Tic Tac Toe game, I set out to finish the Front End project from Free Code Camp, a [Simon Game](https://www.freecodecamp.com/challenges/build-a-simon-game). In essense, there are 4 buttons, from which a sequence will play. Your job is to match that sequence, if you do, the sequence grows. Straight forward, but I really had no clue how to implement this. The first couple of days, I mainly sat down to think about how to implement the game as it had many parts. A little later on, I came up with my first task, which was creating a 'div' that blinked when it was clicked.

## First task - blinking div

This took much longer to accomplish that I had liked. Mainly because when I first saw the [documentation](https://facebook.github.io/react/docs/animation.html) on the main site regarding animation, I felt a bit overwhelmed. A lot of it seemed pretty abstract, not things that I was used to seeing, so I dug a little deeper and found this neat [little tutorial](http://unitstep.net/blog/2015/03/03/using-react-animations-to-transition-between-ui-states/). It had just what I needed, a blinking ```div```. Although this article should have been enough to get things going, things didn't quite click with me as there were things that I was still confused about, specially the CSS bits with 'enter', 'leave' and their active states and so I searched a bit further. I came across this [last article](https://medium.com/@joethedave/achieving-ui-animations-with-react-the-right-way-562fa8a91935#.q6ofquo9i) that really put everything into perspective. Oh, and I also took a look at the [animation docs](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions) on MDN. Yeah, I know, lots of reading.

If somebody was having the same problem, I'd recommend reading the animations docs, followed by the medium article and lastly the tutorial about the blink. Knowing about how transitions work fundamentally will help you grasp the concept, no matter how the API is trying to bend things. 

After playing around for a bit, I ended up with this. 

<p data-height="223" data-theme-id="0" data-slug-hash="wgBQZY" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="Simon game" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/wgBQZY/">Simon game</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

The main takeaway here is that React handles transitions inside its ReactCSSTransitions component. You label elements with a 'key' value and animations will occur when elements with these keys enter or leave the component. The animation was achieved by keeping an array of booleans, and everytime a click occurs this value is toggled, creating a 'leave' effect activating the blink.

Ok, that was task one.

## Second task - playing a sequence of blinks

Now, the next step was animating a sequence of blinks. The first thing I tried was this

~~~
for(var i=0; i<seq.length; i++){
	// component.toggleValues(i)
}

~~~

When I ran this code, the only blink that played was the last one. This happens because each blink call is overriding the other, not letting the first one finish and thus only the last one plays.
I was stumped on this for a while, mainly because I was looking in the wrong place to solve a simple timing problem. I was looking at React-motion, a React library for animation. This seem a bit of a overkill, considering that all I wanted was a sequencing of blinks and I already had the blink. But curious to see if this would solve my problem, I decided to dive in. I read this [article](https://medium.com/@nashvail/a-gentle-introduction-to-react-motion-dc50dd9f2459#.s367bigir), which build a menu that popped out from a button. As interesting as the topic was, I didn't need 90% of it. However, I did find the answer to my problem at the end of the article where there was a bit on creating the delay of the icons showing. 

The javascript that I was missing was this:

~~~
// prints 1, 2, 3, 4, 5 in a timely manner
for(var i=1; i<=5; i++){
  setTimeout(function(i){
    // this is the value being passed, the function
    return function(){
      console.log('printing, ', i);
    }
  }(i), i*1000) // immediately executes by calling it with fn(i)
}
~~~

The setTimout function is used if you want to execute something after an 'x' amount of time. Now, the function benig passed to setTimeout is returning a function. This is a common JS problem where if we didn't have this, the values of i would not freeze and the values printed would all be 6. So to get around it, we pass a function that is executed right away, this returns a function which has access to the unique 'i' value.

And this was exactly was I needed to perform my sequence of blinks, mainly just replacing the print statement with the bit of code that activated the blink.

From this point on, it was quite simple to develop the game further. I was able to add a level up, after the user entered a correct sequence and reset the level on bad input. However, things weren't quite finished. The sequence could play and all, but the user had an infinite amount of time to enter the correct sequence. What's the fun if you aren't being timed? That brought me to my third task, a timer.

## Third task - creating a timer for the user

Although I had implemented a timer before for the previous project, a Pomodoro clock. When it came time to implement it again, I delayed it for as long as possible. Mainly because when I put that code together, I didn't plan on looking back at it, funny how things turn out. It was time to look at that mess. 

From looking over the code, the timer was implemented in this manner:

~~~
function getRemainingTime(endtime){
  var t = Date.parse(endtime) - Date.parse(new Date());
  var seconds = Math.floor((t/1000) % 60);
  var minutes = Math.floor((t/1000/60) % 60);
  
  return {
    'total': t,
    'minutes': minutes,
    'seconds': seconds   
  }
}

var endtime = new Date();
endtime.setSeconds(endtime.getSeconds() + 10);

var timer = setInterval(function(){
  var t = getRemainingTime(endtime);
  console.log('counting ', t.total, t.minutes, t.seconds);
  
  if(t.total == 0){
    console.log('Finished counting');
    clearInterval(timer);
  }
}, 1000);
~~~

The way this works is by first creating a 'deadline' (ie. 10 seconds from now). The next bit that runs is our setInterval, what this does is run the function inside for an 'x' delay which in this case is set to 1000 ms. So here there will be an initial delay before our function executes, by then 't' will start counting down starting at 9. And this function will be called over and over until our timer is exhausted down to 0, which is when it'll stop.
This bit of code is taken from [here](https://www.sitepoint.com/build-javascript-countdown-timer-no-dependencies/), so I highly suggest you take a look as it does a much better job at explaining timers than I do.

After re-reading and understanding this timer mechanism, I proceeded to add this to the game.

The flow of the game now was:

1. Play sequence, following it start a timer that awaits user input
2. If timer runs out, replay the the step above
3. The other two cases that affect timer are on level up, where a new step is added to the sequence and then we go back to step 1 and on wrong input where we go straight back to step 1

Doing this, I also discovered I needed to disable the ability to click the blinks at several steps, other wise there are some craaazy bugs. Race conditions and what not, very ugly stuff. The disables were placed, during sequence animation, after wrong input and after level up.

The disabling was done by setting the CSS property of 'pointer-events' to 'none'.

Almost done, fourth step was adding sound to the clicks.

## Fourth task - adding the sound

Several sounds were given to add to each blinker. Given that they were links, naturally my thought was that I needed to use AJAX to retrieve the sounds first, store them and play them when needed. And so, I set off to learn how to do AJAX calls in React. I read [this article](http://andrewhfarmer.com/react-ajax-best-practices/) and [this one](https://daveceddia.com/ajax-requests-in-react/) too. At the end, I decided to go with 'axios' as it had an example. But this failed, as I was not able to retrieve the sound data, but it did work for regular JSON format data. This was annoying. 

I remember going home frustrated that day, have my progress halted by a stupid AJAX call. However, the next day I managed to figure it out. The answer came from this [StackOverflow question](http://stackoverflow.com/questions/9419263/playing-audio-with-javascript).

~~~
// JS code to load and play audio
var audio = new Audio('audio_file.mp3');
audio.play();
~~~

It turns out that I could just place the source of the mp3 file and this Audio API would do all the heavy lifting for me. Pretty sweet solution considering the frustration on the day prior.

And with this, most of the logic of my game was done. Here's what I had after this point:

<p data-height="341" data-theme-id="0" data-slug-hash="KaMBEm" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="working simon game" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/KaMBEm/">working simon game</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## Fifth task - Polishing the User Interface

This is always the most dreaded task, ha. In this case, what I needed to do was straight forward. First create the quarters, then create the set of controls and finally wired up everything. Creating the quarters was something new, and was able to do it thanks to [this JS Fiddle](http://jsfiddle.net/cardeo/8ku6T/). The circular control board was a bit more tedious to do as everything little aspect of it required custom sizing. This ended up being alot of ID tagging and a lot of repetitive CSS, especially centering, I've become really good at centering text and blocks.

This is the final product:

<p data-height="416" data-theme-id="0" data-slug-hash="xgrpbw" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="Simon game" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/xgrpbw/">Simon game</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

One of the reasons, why it is a bit oversized is that when I was developing this on my CodePen, I had my browser size set to 75%. This is mainly because I can see about 10 more lines of code on the editor this way. But yes, I goofed up. However, it doesn't look too bad when it is fully expanded on a browser.

## Conclusion

This last Free Code Camp project was one of the toughest for sure. Highly due to the research aspect of it, not knowing how to do many things and oftentimes getting trying to find the answer. But I'm glad that I decided to stick with things and eventually I did finish it up. :)


