---
layout: post
title: Sudoku solver
comments: false
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

The code to this can be found [here](https://github.com/danielcodes/practice-problems/blob/master/epi/16-recursion/sudokuSolver.py).



- give small summary
- post link to code on github
- show what was the roadblock that you ran into, show examples and explain



