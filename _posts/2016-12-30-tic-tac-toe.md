---
layout: post
title: Building a Tic Tac Toe game
comments: true
---

In the past two weeks, I have been working on a Tic Tac Toe game for Free Code Camp. This one took a couple of sittings as it required me to learn quite a few things. I chose to use React for the project as it is something that I've been meaning to learn for the longest time and had some momentum coming in after finishing my Pomodoro Clock in React. 

The user stories for the project were simple:

* I can play a game of Tic Tac Toe with the computer.
* My game will reset as soon as it's over so I can play again.
* I can choose whether I want to play as X or O.

Not too bad, have a choice between X or O, create an AI and reset when the game ends.

## Starting out

The first thing I did was prototype the user interface, which was just a grid. After this, I soon found that I had put together a huge component and didn't know how to break it up. Also, if I had kept that grid component, each square would need to have an ```onClick``` handle attached to it to display X or O on that square. So soon and things were already starting to look ugly. I needed to break down this HTML. Luckily, I happened to come across the tutorial on [React's main site](https://facebook.github.io/react/tutorial/tutorial.html), which was a Tic-Tac-Toe game.

## Going through the tutorial

This tutorial was a lot of fun, I highly recommend it. I found the main thing that I needed, which was to break the components apart. They did this by creating several components such as a Game, a Board and a Square. You start small with all the functionality contained in the Square and refactor up. This means the parent component contains the data and passes a function down to the child for it to modify this data. This was the only thing I needed, but there were other goodies in the tutorial, such as creating a history of all the moves and being able to move back and start playing from there.

After finishing the tutorial, there were additional features that you could implement if you wanted to test your knowledge. Since I wanted to learn, I did them. 

<p data-height="306" data-theme-id="0" data-slug-hash="LbBMyK" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="Tic Tac Toe new features" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/LbBMyK/">Tic Tac Toe new features</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

The most noticeable additions are, bolding the current move, the button to sort the moves and highlighting the winning line.

## Back to my prototype

Armed with this new knowledge, I proceeded to add these separated components to my own game. At this point, I thought that I was ready to start looking into creating the AI to play the game.

## Thinking about the AI

The first idea that came to mind to create this AI, was to have the computer capture the center or corner on its first move. From here, look for threats which is when the opposite player has two pieces lined up. A couple of limitations with this approach:

~~~
Here, there's no threat right away, but if it play its move on either of the T spots, it is going to lose.

 |T|O
-----
 |X|T
-----
X| |
~~~

This is when I realized that I couldn't really come up with a few simple checks to solve this problem.
After some research, I found that there was an algorithm that I could specifically use for these kind of turn taking games, **Minimax**.

## Minimax

I got to understand this algorithm from two main sources:

* [Article](http://neverstopbuilding.com/minimax)
* [Video](https://www.youtube.com/watch?v=CwziaVrM_vc)

The article explains in depth how Minimax works, and the second is a video that actually implements the algorithm, done in ```C++```.

## Back to my project

However, even though I understood the concept, I had a bit of trouble getting started on writing the algorithm. The main roadblock was that I needed to extend my two player version to one where I played against the computer. So I went and created a dumb AI that placed a move anywhere on the board. Now, the goal was clear, I just needed to update this one function so it used Minimax to pick the best move instead of the dumb AI.

## Understading Minimax

Essentially, Minimax is a brute-force algorithm that explores every possible option. It does this by playing out all scenarios and giving each square a score. The score can be of 3 kinds, **positive for a win, 0 for a draw and negative for a loss**. At the end, you pick the highest score and play that move.

I won't go into much detail as to how the recursion works, as the two sources above can do a much better job at explaining.

But, I do want to mention a small optimization that I made to the algorithm. Since Minimax is a brute force algorithm, it is really slow. When I first wrote the function, it took a good 30 seconds to finish computing on an empty board, and this was running locally on node. No doubt this was gonna crash my browser. 

To see this, think about the tree, 

~~~
First choice,                  (9 options)
                            |         | | | | | | | | 
Second choice,         (8 options)      ...
                     |        | | | | | | | 
Third choice,   (7 options)             ...     
                  |     | | | | | | 
		...       ...
~~~

If the computer runs the algorithm from the beginning, it first has 9 options and each one of these expand into a tree that each have 8 options each, subsequently these 8 nodes each expand into 7 trees of their own. Try to draw it, to get a feel for the sheer magnitude of the tree. 

## First move doesn't help

Running the algorithm on an empty board returns a score of **0 for each of the squares**. 

~~~
 | |    | |X    | |X
-----  -----   -----
 | |    | |     |O|
-----  -----   -----
 | |    | |     | |
~~~

Say the AI plays a corner, it then runs the algorithm and finds that the center is the only safe move so it plays that and eventually brings the game to a draw. It does this for each of the other 9 squares. No matter where the first move is placed, the second player can always tie the game. Thus, the zero scores for all 9 squares.

## Optimizing Minimax

As explained, the first computation is completely unnecessary as there's no go-to move that will guarantee a win and as explained it is very expensive. And so to save some computation time, I optimized for the first and second move, if the AI has the first move, it either plays it in one of the corners or the center. Given the first move, you can actually play anywhere and tie all the time but if you play a corner or the center and the opponent (you) goof up, you'll end up losing. As for the second move, I made it so that it captures either the center or a corner if the center is taken since these two spots will prevent the other from winning.

Getting the first or second move fast makes the tree start from the node with 7 options. Thankfully, the browser seemed to be able to handle this computation just fine. 

## Putting it all together

After creating this function, I just had to add it to my component. This was easy enough to do.

<p data-height="553" data-theme-id="0" data-slug-hash="MbVXLv" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="Tic Tac Toe" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/MbVXLv/">Tic Tac Toe</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

This was the end result, now the final step was to style it up.

## Final touches

Phew, most of the logic had been wrapped up at this point, now the task was to make it appealing to users.
Right from the get-go I thought of using a modal to first prompt the user for input, select an icon (X or O) and who goes first. Then, start the game and reset back to the modal when the game finished.

But I had no idea how to implement this modal. Thankfully, I found this [tutorial](https://daveceddia.com/open-modal-in-react/), that had just what I needed.
After adding some color, and rewiring things here and there, this was the end result.

<p data-height="573" data-theme-id="0" data-slug-hash="qqzbyd" data-default-tab="result" data-user="danielcodes" data-embed-version="2" data-pen-title="Tic Tac Toe" class="codepen">See the Pen <a href="http://codepen.io/danielcodes/pen/qqzbyd/">Tic Tac Toe</a> by Daniel Chia (<a href="http://codepen.io/danielcodes">@danielcodes</a>) on <a href="http://codepen.io">CodePen</a>.</p>
<script async src="https://production-assets.codepen.io/assets/embed/ei.js"></script>

## Conclusion

It was a fun project overall. It definitely tested my patience, from not knowing how to break down the big grid, having to learn about a new algorithm and creating the user interface. It was a great reminder that projects take time and that small victories count as the final product is always worth seeing.

And that's a wrap :)

PS. Since this was done in a CodePen, I ended up with 400 lines of JS. Time to move this locally and give the project some structure.

