---
layout: post
title: Building a Simon game
comments: false
---

After finishing the Tic Tac Toe game, I set out to finish the last Front End project from Free Code Camp, a [Simon Game](https://www.freecodecamp.com/challenges/build-a-simon-game). The game consists of 4 buttons, from which a sequence will play. The player's job is to match that sequence. If you do, the sequence grows otherwise it plays the same sequence until you pass the current level. Straight forward rules, but I really had no clue how to implement this. The first couple of sessions, I mainly sat down to think about the structure of the game as it had many parts. A little later on, I came up with my first task, which was creating a ```div``` that blinked when it was clicked.

## First task - Blinking div

This took much longer to finish than I had liked. Mainly because when I first saw the [documentation](https://facebook.github.io/react/docs/animation.html) on the main site regarding animation, I felt a bit overwhelmed. A lot of it seemed pretty abstract, not things that I was used to seeing. After some digging, I found this neat [little tutorial](http://unitstep.net/blog/2015/03/03/using-react-animations-to-transition-between-ui-states/). It had just what I needed, a blinking ```div```. Although this article should have been enough to get things going, things didn't quite click with me as I was still confused about certain areas, specially the CSS bits with ```enter```, ```leave``` and their active states and so I searched a bit further. I came across this [last article](https://medium.com/@joethedave/achieving-ui-animations-with-react-the-right-way-562fa8a91935#.q6ofquo9i) that really put everything into perspective. This included a link to the first thing I should've looked at, the [animation docs](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Transitions/Using_CSS_transitions) on MDN. 

If somebody was to start this project, I'd recommend reading the animations docs, followed by the medium article and lastly the tutorial about the blink. Knowing about how transitions work fundamentally will help you grasp the concept, no matter how other APIs are trying to bend things. 

I don't have an example to show as, after finishing the blink, I did not save a copy and built the rest of the project on top of it. But there is a a link [here](http://jsfiddle.net/pchng/17rq3s6d/1/), which is more or less what I ended up with.

The main takeaway here is that React handles transitions inside its ```ReactCSSTransitions``` component. You label elements with a ```key``` value and animations will occur when elements with these keys enter or leave the component. The animation was achieved by keeping an array of booleans, and everytime a click occurred this value was toggled, creating a leave effect activating the blink.

Ok, that was task one.

## Second task - Playing a sequence of blinks

Now, the next step was animating a sequence of blinks. The first thing I tried was this

~~~
for(var i=0; i<seq.length; i++){
	// component.toggleValues(i)
}

~~~

When I ran this code, the only blink that played was the last one. This happens because each blink call is overriding the other, not letting the first one finish and thus only the last one plays.
I was stumped on this for a while, mainly because I was looking in the wrong place to solve a simple timing problem. I was looking at React-motion, a React library for animation. This seemed a bit of an overkill, considering that all I wanted was a sequence of blinks and I already had the blink. But curious to see if this would solve my problem, I decided to dive in. I read this lengthy [article](https://medium.com/@nashvail/a-gentle-introduction-to-react-motion-dc50dd9f2459#.s367bigir), which built a menu that popped out from a button. As interesting as the topic was, I didn't need 90% of it. However, I did find the answer to my problem at the end of the article where there was a bit on creating the delay of the icons popping up. 

The javascript that I was missing was this:

~~~
// prints 1, 2, 3, 4, 5 in a timely manner
for(var i=1; i<=5; i++){
  setTimeout(function(i){
    return function(){
      console.log('printing, ', i);
    }
  }(i), i*1000) // immediately executes by calling it with fn(i)
}
~~~

The ```setTimout``` function is used if you want to execute something after an **x** amount of time. Now, the function benig passed to ```setTimeout``` is returning a function. This is a common JS problem where if we didn't have this, the values of ```i``` would not freeze and the values printed would all be 6. So to get around it, we pass a function that is executed right away, this returns a function which each with the unique ```i``` value.

And this was exactly was I needed to perform my sequence of blinks, mainly just replacing the print statement with the bit of code that activated the blink.

From this point on, it was quite simple to develop the game further. I was able to add a level up, after the user entered a correct sequence and reset the level on bad input. But, I wasn't done. The sequence could play and all, but the user had an infinite amount of time to enter the correct sequence. What's the fun if you aren't being timed? That brought me to my third task, a timer.

## Third task - Creating a timer for the user

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

The way this works is by first creating a ```deadline``` (ie. 10 seconds from now). The next bit that runs is our ```setInterval```, what this does is run the function inside for an **x** delay which in this case is set to 1000 ms. So here there will be an initial delay before our function executes, by then ```t``` will start counting down from 9. And this function will be called over and over until our timer is exhausted down to 0, which is when it'll stop.
This bit of code is taken from [here](https://www.sitepoint.com/build-javascript-countdown-timer-no-dependencies/), so I highly suggest you take a look as it does a much better job at explaining timers than I do.

After re-reading and understanding this timer mechanism, I proceeded to add this to the game.

The flow of the game now was:

1. Play sequence, following it start a timer that awaits user input
2. If timer runs out, replay the the step above
3. The other **two cases** that affect timer are on level up, where a new step is added to the sequence and then we go back to **step 1** and on wrong input where we go straight back to **step 1**

While doing this, I discovered I needed to disable the ability to click the blinks at several steps, other wise there are some craaazy bugs. Race conditions and what not, very ugly stuff. The disables were placed on three places, during sequence animation, after wrong input and after level up. This was done by setting the CSS property of ```pointer-events``` to ```none```.

Almost there, fourth step was adding sound to the clicks.

## Fourth task - Adding the sound

Several sounds were given to add to each blinker. Given that they were links, naturally my thought was that I needed to use ```AJAX``` to retrieve the sounds first, store them and play them when needed. And so, I set off to learn how to do AJAX calls in React. I read [this article](http://andrewhfarmer.com/react-ajax-best-practices/) and [this one](https://daveceddia.com/ajax-requests-in-react/) too. At the end, I decided to go with 'axios' as it had an example. But this failed, as I was not able to retrieve the sound data, but it did work for regular JSON format data. This was annoying. 

I remember going home frustrated that day, have my progress halted by a stupid AJAX call. However, the next day I managed to figure it out. The answer came from this [StackOverflow question](http://stackoverflow.com/questions/9419263/playing-audio-with-javascript).

~~~
// JS code to load and play audio
var audio = new Audio('audio_file.mp3');
audio.play();
~~~

It turns out that I could just place the source of the mp3 file and the ```Audio API``` would do all the heavy lifting for me. Pretty sweet solution considering the frustration the day prior.

And with this, most of the logic of my game was done. Here's what I had after this point:

<p data-height="341" data-theme-id="0" data-slug-hash="KaMBEm" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="working simon game" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/KaMBEm/">working simon game</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## Fifth task - Polishing the User Interface

This is always the most dreaded task, ha. In this case, what I needed to do was straight forward. First create the quarters, then create the set of controls and finally wire up everything. Creating the quarters was something new, and was able to do it thanks to [this JS Fiddle](http://jsfiddle.net/cardeo/8ku6T/). The circular control board was a bit more tedious to do as every little aspect of it required custom sizing. This ended up being alot of ID tagging and a lot of repetitive CSS, especially centering, I've become really good at centering text and blocks.

This is the final product:

<p data-height="416" data-theme-id="0" data-slug-hash="xgrpbw" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="Simon game" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/xgrpbw/">Simon game</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

One of the reasons, why it is a bit oversized is that when I was developing this on my CodePen, I had my browser size set to 75%. I set it up this way because I can see about 10 more lines of code on the editor. I done goofed up, but overall it is not too bad :).

## Conclusion

This last Free Code Camp project was one of the toughest for sure. Highly due to the research aspect of it, not knowing how to do many things and oftentimes getting stuck trying to find the answer. But I'm glad that I decided to stick with things and eventually finishing it up. :)

