---
layout: post
title: Reading Python, a formatted table
comments: true
---

After finshing the __Python Fundamentals__ course on Pluralsight, the next step was the follow-up course, __Python - Beyond the Basics__. I recently went over the module about string and representations, which covered ```str()``` and ```repr()```. Both methods when called on, provide information about the object instance. ```str()``` providing a general description (contents) while ```repr()``` provides a fuller description (type and other small details). The module was short and sweet, about 20 minutes long. However, near the end of the course, a program was introduced that created tables based on provided lists. And this Table class had ```str()``` and ```repr()``` methods defined. The ```str()``` method provided the table contents in a nice formatted output, while ```repr()``` simply provided the type and column titles.

```python
#Defining columns and data lists
header = ['First Name', 'Last Name']
first = ['Daniel', 'Ronnie', 'Vanessa']
last = ['Chia', 'Hernandez', 'Luka']

t = Table(header, first, last)

print(str(t))
print(repr(t))

#Output
#a formatted table for str()
First Name Last Name
========== =========
Daniel     Chia     
Ronnie     Hernandez
Vanessa    Luka     

#type and column titles for repr()
Table(header=['First Name', 'Last Name'])
```

As an exercise I decided to dive into the code and attempt to decipher what was being written. What followed was a painful time trying to understand the code. I don't know Pluralsight's policy on putting their content out in the open, so I won't put up all the code, only snippets of it. Explaining whatever stumped me, so this post is to:

* reinforce my learning
* show insight to those who haven't seen these practices

Without further ado, let's begin:

As you can see above, we're passing a couple of lists (a header for the column titles, followed by lists of data) to the the Table class. This data is then manipulated in the ```str()``` method to provide the formatted output shown.  

The breakdown of the ```str()``` function was actually quite simple:

First, a few variables are defined:

1. number of columns
2. widths of the colums, placed in a list
   * compare the longest string in a column vs. column title, the higher of the two becomes the width for that column  
3. string formatters, to provide space padding for strings 

The 3rd defined variable, provides a list of the string formatters, ``` [{:column_width}, {:column_width_2}, .. ] ```

```python
#padding, example
first_name = '{:10}'.format('Daniel')

'''
output -> 'Daniel    '
'''
``` 

After setting up those variables, the table is built row by row.

First an empty list is created, then 3 append calls are made, one for the header, one for the = line separators and lastly pair up the data lists and place them row by row. Each row is then tied together with a space (column divider), and returned with new line separators for a tabular display. 

I provide the code to create the table, you can get it from:

```sh
git clone https://gist.github.com/9840c91f8e94e5177aec.git table
python3 table/table.py
```

First the empty list,

```python
#empyt list
result = []
```

Next, append the headers,

```python
#append header, a generator expression 
#formatters is the list with {:col_width}
#ie. {:10}
#header is a list of column titles
#ie. ['First name', 'Last name']
'''
outputs -> ('First name', 'Last Name')
note: You don't actually see this, it is hidden in a generator expression, unless you decide to iterate over it. Note that the expression is inside the result list, [(gen. expr), ..]
'''
result.append(
	formatters[i].format(header[i])
	for i in range(col_count))

```

the line separators (=),

```python
#append = line separator, also a generator expression
#grab the widths, ie. [10, 9]
'''
outputs -> ('==========', '=========')
note: can't see this either
'''
result.append(
	('=' * col_widths[i]
	 for i in range(col_count)))

```

creating the rows based on the provided data lists,

```python
#append data from lists (first and last names), tricky part
#data is a tuple of lists 
#ie. ( [first_names], [last_names], etc )
#by using zip and the * operator, we're unpacking this tuple and creating the pairs
#we end up with [(first_name, last_name), (another_fn, another_ln), ..], which are the rows themselves!
#we're giving the appropiate padding to each string
'''
outputs -> ['Daniel    ', 'Chia     '],
		   ['Ronnie    ', 'Hernandez'], 
		   ['Vanessa   ', 'Luka     ']
'''

for row in zip(*data):
	result.append(
		[formatters[i].format(row[i])
		 for i in range(col_count)])
```

and tying everything together.

```python
#at this point our result list is almost complete
#it looks like this, printing the items inside it

'''
[ ('First name', 'Last Name')
  ('==========', '=========')
  ['Daniel    ', 'Chia     '] 
  ['Ronnie    ', 'Hernandez']
  ['Vanessa   ', 'Luka     '] ]
'''

#join the rows with spaces 
result = (' '.join(r) for r in result)

#return the list with line separators added
#voila! the list is done
return '\n'.join(rslt)
```

Looking back at the code, it doesn't seem so bad. It's often lack of understanding that brings those 'wat' moments, so it's worthwhile to spend the time understading concepts. Read and read more code. Narrowing down on the concepts that threw me off a bit, there are:

* lists comprehensions ```[]``` vs. generator expressions ```()```
  * these are similar in providing a group a values
  * some differences are that gen. expressions take less memory and cannot be accessed by index
  * usage, for several iterations use a list, one time use, a generator expression
* zip function, unpacking arguments with *
  * passing the * operator to ```zip()```, unpacks whatever arguments you throw at it
  * in the example above, data is a tuple of lists (first, last) and we want to create rows out of that data. Without *, the lists would have been needed to be passed indidually to zip, ```zip(list1, list2, ...)```
* escaping the ```{}``` on format method
  * in order to create a list of formatters ```{:col_widths}```, the ```{``` character needed to be escaped
  * more info in the [official docs](https://docs.python.org/2/library/string.html#format-string-syntax)


###Conclusion
Reading someone else's code can be a humbling experience. It can be tough to break down the complexity of the code or peeling the layers, but at the same time, it is extremely rewarding. The __Aha, I GOT IT!__ moment. Using pen and paper to draw out the problem definitely helped in reducing the logic that I had to wrestle with in my mind.


###References
[1] [Generator expressions](http://code-maven.com/list-comprehension-vs-generator-expression)

[2] Zip, [here](http://stackoverflow.com/questions/2511300/why-does-x-y-zipzipa-b-work-in-python) and [here](http://pavdmyt.com/python-zip-fu/)


