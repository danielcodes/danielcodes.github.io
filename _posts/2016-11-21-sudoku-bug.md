---
layout: post
title: Sudoku bug
comments: true
---

The other day I was trying to implement a Sudoku Solver, specifically the one from Elements of Programming Interviews. My usual approach to these type of problems has been to write down the sudo code on paper, step through it and if I understand it, I can usually implement it without looking at the solution. This was certainly the case with this Sudoku Solver, except that I missed a crucial keyword in the program that created a bug that sent everything spiraling out of control.

### Pseudo-code

First, let me explain how this Sudoku Solver is supposed to work on a step-by-step basis.

1. Iterate through each slot in the grid
2. Two base cases
  * The grid has been filled both horizontally and vertically
  * The slot already has a value
3. If it doesn't fall into the two categories above, the program proceeds to attempt all values from 1-9
4. If the value is valid, set the value and recurse, if this recursion passes, return True
5. If the loop above terminates, undo the assignment (backtrack) and return False

One thing to note is that this solves the puzzle in a vertical manner, filling in the columns and moving towards the right.

The code to this can be found [here](https://github.com/danielcodes/practice-problems/blob/master/epi/16-recursion/sudokuSolver.py). Take a glance at it since I'll be discussing it in the snippets below.

### Where it all went wrong

This looks implementation doesn't look too bad. However, I ran into a pretty nasty bug. On the case base case where the slot already has a value, I wrote:

~~~ python
# recurse onto the next row
if grid[i][j] != 0:
    sudokuSolverHelper(i+1, j, grid)	
~~~

##### So, what's wrong with this?

The problem lies when this call gets returned, say it recurses down to the next value and all values from 1-9 are not valid. This returns ```False``` but the rest of the code continues to execute. This means that the code then attempts to try to use all values from 1-9 on this already filled-in value. No bueno.

### The solution

~~~ python
# add return prior to the recursion
if grid[i][j] != 0:
    return sudokuSolverHelper(i+1, j, grid)	
~~~

It's hard to see how this helps right away, so let's run through an example:

~~~
_ empty
5 pre-filled 5
_ empty
~~~

Say the first empty value is a 2, it recurses and goes to 5 which hits our 

```return rec(...)``` 

case, now we're at the second empty slot, then say after attempting all values on the second empty slot, False is returned. But now since we have the return statement, the code below it will not run and this same False value will be given to the recursive step when 2 was the attempted value.

### In short

That missing ```return``` caused the pre-filled values to be overwritten, adding it prevents it.

PS. Copy the code and go to [repl.it](https://repl.it/languages/python) and try it out, adding and removing the return keyword


