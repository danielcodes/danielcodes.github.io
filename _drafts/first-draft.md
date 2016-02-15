---
layout: post
title: Playing with GET/POST on Flask
comments: true
---

I signed up to give a web development workshop for my school's ACM club. Of course, beginning with the basics HTML/CSS to creating a small application with Flask. Planning this has been a little tougher than expected as to what I want people to come out the workshop knowing. Of course, the musts are the **main three elements** that go into a web framework: 

* **URL handling** 
* **Templating** 
* **Object Relational Mapper (ORM)** 

With Flask, one and two are simple, the third one is a little trickier as an extension is needed. After going through the quickstart tutorial for **Flask-sqlalchemy**, I was confident that I'd be able to produce some useful **GET/POST** examples. What I found was that these methods can be abused.

### The GET method

**GET** is the default http method when accessing a page, you're retrieving a page. However, ```GET``` can also be triggered by a form or by a query string in the URL.

#### GET Form
```html
<form action="/get_something" method="GET">
	<input type="text" name="name" >
	<input type="text" name="job" >
	<input type="submit" value="Add Person">
</form>
```

#### GET Query string
```
http://anrandomurl.com/get_something?name=daniel&job=student
```

These are the two ways to pass parameters to a GET request, the way to retrieve them on Flask is by accessing the ```args``` attribute, which returns a dictionary, containing the the key-value pairs passed. In this case, name and job.

```python
#Accessing the parameters
#the whole thing, a dict
argumest = request.args

#a specific parameter
name = request.args.get('name')
```

### The POST method













### References
* []()
* [What is a web framework?](https://www.jeffknupp.com/blog/2014/03/03/what-is-a-web-framework/)
* [Flask-sqlalchemy](http://flask-sqlalchemy.pocoo.org/2.1/quickstart/)

