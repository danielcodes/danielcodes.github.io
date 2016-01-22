---
layout: post
title: Reading Python
comments: true
---

-add `` quotes for all str and repr
-delete wat.py and table.py 
My list of WTF's
first rediscovering generator expressions
finding about zip unpacking
escaping the { with a dobule {
creating the padding


I have moved on from the __Python Fundamentals__ course on Pluralsight and moved on to __Python - Beyong the Basics__. Today I went over the module about string and representations, which covered ```str()``` and ```repr()```. Both methods when called on provide information about the object instance, str() providing a general description (contents) while repr() provides a fuller description (type, other small details). The module was short and sweet, about 20 minutes long. However, near the end of the course, a program was introduced that created tables based on provided lists. And this Table class had str() and repr() methods defined. The str() method provided the table contents in nice formatted output, while repr() simply provided the type and column titles.

```python
#Defining columns
header = ['First Name', 'Last Name']
first = ['Daniel', 'Ronnie', 'Vanessa']
last = ['Chia', 'Hernandez', 'Luka']

t = Table(header, first, last)

print(str(t))
print(repr(t))

#Output
#a nicely formatted table for str()
First Name Last Name
========== =========
Daniel     Chia     
Ronnie     Hernandez
Vanessa    Luka     

#type and column titles for repr()
Table(header=['First Name', 'Last Name'])
```

As an exercise I decided to dive into the code and attempt the decipher what was being written. What followed was a painful time trying to understand the code. I don't know Pluralsight's policy on putting their content out in the open, so I won't put up all the code, only snippets of it. However, I will explain what stumped me while reading the code, so this post is to:

* reinforce my learning
* show insight to those who haven't seen these practices

Without further ado, let's begin:

As you can see above, we're passing a couple of lists (a header for the column titles, followed by lists of data) to the the Table class. This data is then manipulated in the ```str()``` method to provide the formatted output shown.  

After the struggle that I went through to understand the code, looking at it the second time, the logic is actually quite simple. 

First, a few variables are defined:

1. number of columns
2. widths of the colums, placed in a list
   * compare the longest string in a column vs. column title, the higher of the two becomes the width for that column  
3. provide a list for which we can do some string formatting (placing a string in a column with necessary padding), see below 

The 3rd defined variable, provides a list of the string formatters, ``` [{:column_width}, {:..}, .. ] ```

```python

#padding, example
first_name = '{:10}'.format('Daniel')

#output -> 'Daniel    '
``` 

After setting up those variables, everything else is pretty straight forward, the table is built row by row.

First an empty list is created, then 3 append calls are made, one for the header, one for the = line separators and lastly pair up the data lists and place them row by row.

```python
#Note, I suggest you fire up a python shell and test snippets yourself
#I use the ipython shell, running python3 | or just type python3
#empyt list
result = []

#append header, a generator expression 
#formatters is the list with {:col_width}, ie. {:10}
#header is a list of column titles, ie. ['First name', 'Last name']
#done for each column
#outputs -> ('First name', 'Last Name')
result.append(
	formatters[i].format(self.header[i])
	for i in range(col_count))

#append = line separator, also a generator expression
#grab the widths, ie. [10, 9]
#outputs -> ('==========', '=========')
result.append(
	('=' * col_widths[i]
	 for i in range(col_count)))

#append data from lists (first and last names), tricky part
#self.data is a tuple of the data lists, ie. ( [first_names], [last_names], etc )
#by using zip and the * operator, we're unpacking this tuple and creating the pairs
#we end up with [(first_name, last_name), (another_fn, another_ln), ..], which are the rows themselves!
#next iterate through each pair and append to result
#outputs -> [ ['Daniel    ', 'Chia     '], ['Ronnie    ', 'Hernandez'], ['Vanessa   ', 'Luka     '] ] , notice lists!
for row in zip(*self.data):
	result.append(
		[formatters[i].format(row[i])
		 for i in range(col_count)])


#at this point our result list is almost complete
#it looks like this, printing the items inside it

#('First name', 'Last Name')
#('==========', '=========')
#['Daniel    ', 'Chia     '] 
#['Ronnie    ', 'Hernandez']
#['Vanessa   ', 'Luka     '] 

#join the rows with spaces 
result = (' '.join(r) for r in result)

#return the list with line separators added
#voila! the list is done
return '\n'.join(rslt)

```

-finish this tmr, what confuse you? + read over
provide what the table looked like for you when debugging




source
stack overflow for list vs gen
the article
zip with *


