---
layout: post
title: Playing with GET/POST on Flask
comments: true
---

I signed up to give a web development workshop for my school's ACM club. Beginning with the basics HTML/CSS to creating a small application with Flask. Planning this has been a little tougher than expected as to what I want people to come out the workshop knowing. Of course, the musts are the **main three elements** that go into a web framework: 

* **URL handling** 
* **Templating** 
* **Object Relational Mapper (ORM)** 

With Flask, one and two are simple, the third one is a little trickier as an extension is needed. After going through the quickstart tutorial for **Flask-sqlalchemy**, I was confident that I'd be able to produce some useful **GET/POST** examples. What I found was that these methods could be used interchangeably and abused. Here's why:

### The GET method

> **GET** is the default http method when accessing a page, you're retrieving a page. 

```GET``` can be passed arguments by a form or by a query string in the URL.

#### GET Form
~~~
<form action="/get_something" method="GET">
	<input type="text" name="name" >
	<input type="text" name="job" >
	<input type="submit" value="Add Person">
</form>
~~~

#### GET Query string
~~~
http://arandomurl.com/get_something?name=daniel&job=student
the ? is followed by key=value pairs tied together with an &
~~~

These are the two ways to pass arguments to a GET request, the way to retrieve them on Flask is by accessing the ```args``` attribute, which returns a dictionary, containing the the key-value pairs passed. In this case, name and job.

~~~
#a dict
#check out the reference below for more on python dictionaries
arguments = request.args

#a specific argument 
name = request.args.get('name') 
#or
name = request.args['name']

#Note: if argument is not there, get returns None, access with brackets gives a KeyError
~~~

### The POST method

> **POST** is the http method used when you want to alter the application.

ie. creating a new account, saving records to a database.

#### POST Form
~~~
<form action="/post_something" method="POST">
	<input type="text" name="name" >
	<input type="text" name="job" >
	<input type="submit" value="Add Person">
</form>
~~~

When this form is submitted, the following passed arguments, name and job can be accessed in the following manner:

~~~
#a dict
arguments = request.form

#a specific argument 
name = request.form.get('name')
#or
name = request.form['name']

#Note: if argument is not there, get returns None, access with brackets gives a KeyError
~~~

### Abusing GET/POST
So far, I've shown that both **GET** and **POST** can take arguments in pretty much the same manner. Difference is that one receives it in ```args``` while the other in ```form```. In Flask, it looks a little like this:

~~~
@app.route('/<some-url>', methods=['GET', 'POST'])
def some_view():
	#depending on what method you used to pass arguments
	#retrieve them from args or form and do something

    return <some_html> 
~~~

### Conclusion
Since you can pass arguments to either method, they can be used interchangeably and therefore abused. To prevent this, **be informed** as to what GET and POST are. GET should only retrieves information, while POST is used to create new information.


### References
* [Python Dictionaries](https://www.jeffknupp.com/blog/2015/08/30/python-dictionaries/)
* [What is a web framework?](https://www.jeffknupp.com/blog/2014/03/03/what-is-a-web-framework/)
* [Flask-sqlalchemy](http://flask-sqlalchemy.pocoo.org/2.1/quickstart/)
