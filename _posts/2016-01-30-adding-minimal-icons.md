---
layout: post
title: Adding minimal social media icons
comments: true
---

I set out to add a list of social media icons this past week, I had been postponing the task for a while now and finally got around doing it. It shouldn't have been hard, find icon images and link them up, right? Well.. things didn't go so smoothly for me. Actually, it did work, but there was a pesky little aesthetic detail that drove me a bit crazy.

I decided to use [Font Awesome](https://fortawesome.github.io/Font-Awesome/) for the icons, from its vast collection of icons, I only needed 4. But who knows later on I might create other profiles that might require them, it's nice to have options.

I chose to be lazy and simply added the provided CDN to my site:

~~~
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/font-awesome/4.5.0/css/font-awesome.min.css">
~~~

With that in place, icon insertion is just a matter of placing ```<i>``` tags, selecting the ones you want through a class:

~~~
<i class="fa fa-github"></i>
~~~

So I had the icons, the only extra thing that I did was wrap these tags with ```<a>``` tags to link them to my profiles. Check it out in the JSFiddle below:

<iframe width="100%" height="200" src="//jsfiddle.net/m2s2qshm/3/embedded/html,result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

### The Problem

At this point, I had something working, icons and their respective links. Now, if you look at the result tab in the fiddle, you'll see that the first three icons have some type of dash shadow at the bottom. Whenever I'd hover over these icons, the shadow would show. I can only assume that it has to do with links themselves, similar to how hovering over text links makes them darker and underlined.

I looked at other tutorials and it turned out that they placed all the links in a ```<ul>``` tag. I proceded to play around with things, but it must of not been my day as it did not work. I was pretty close to giving up on the issue altogether.

### Solution

Today, out of curiosity I looked over a repository that a friend starred on GitHub. I visited the [website](https://nusmods.com/timetable/2015-2016/sem2) and saw that they had the icons I wanted. I decided to give it one last ditch effort. I opened up Developer Tools and proceeded to look at the markup. I saw that the site used a ```<ul>``` tag, and I wondered, 

>Why the heck didn't it work the other time?

Whatever the case may be. I proceeded to wrap the links in ```<li>``` tags and the whole thing in a ```<ul>``` tag.

Only two things remained to be done, remove the bullet points from the list and place the elements inline, done with the following bit of CSS:

~~~
/*targetting the <ul> with social-icons class*/
.social-icons { list-style: none; }
.social-icons li { display: inline-block; }
~~~

Here's the final result:

<iframe width="100%" height="150" src="//jsfiddle.net/kn3y78gz/2/embedded/result/" allowfullscreen="allowfullscreen" frameborder="0"></iframe>

No more annoying little dashes at the bottom, good grief.

