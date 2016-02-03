---
layout: post
title: Set your Disqus configuration variables 
comments: true
---

After deciding on the theme that I wanted to use for this site, [Hyde](https://github.com/poole/hyde). I was also actively looking for ways to improve my site. I found this really helpful [article](http://joshualande.com/jekyll-github-pages-poole/), which helped me add an __Archive__ to see my posts in log format, __Analytics__ to track views and __Comments__ for feedback. After going through the steps, everything was well set up, or so I thought. When setting up Disqus comments, I placed the provided markup on my page but neglected to fill in the variables that they had **strongly** suggested me to replace.

### The variables, url and identifier

```javascript
var disqus_config = function () {
	// Replace PAGE_URL with your page's canonical URL variable
	this.page.url = PAGE_URL;

	// Replace PAGE_IDENTIFIER with your page's unique identifier variable
	this.page.identifier = PAGE_IDENTIFIER;  
};
``` 

### Where I goofed up

The way Disqus works is by keying a provided URL, defined by a url variable. As you can tell, I did not define this URL and when I received the first comment on my page, sometimes it showed up and sometimes it didn't. The problem was how the page was being accessed, by that I mean the protocol used. Disqus was creating two different comment threads for ```http://``` and ```https://```. And so, I got annoyed and had to fix the problem.

I headed over to the documentation and found that I needed to provide an absolute url, and the slug part of the url that comes after the domain name.

```javascript
//ie. example url and identifier
var this.page.url = "http://danielcodes.github.io/2016/01/12/resuming-python/"
var this.page.identifier = "/2016/01/12/resuming-python/"
```

The way to obtain these variables with Jekyll is:

```javascript
{% raw %}
// Replace PAGE_URL with your page's canonical URL variable
var this.page.url = "{{site.url}}{{page.url}}";

// Replace PAGE_IDENTIFIER with your page's unique identifier variable
var this.page.identifier = "{{page.url}}";
{% endraw %}
```

The beginning part, ```site.url``` has to be defined in your ```_config.yml```, check mine [here](https://github.com/danielcodes/danielcodes.github.io/blob/master/_config.yml). This is followed by, ```page.url```, which will look for the route of your blog post.

That's pretty much it, define your javascript cofiguration variables and avoid the split threads.
And don't forget to test things out first!

### References

* [Avoiding split threads and missing comments](https://help.disqus.com/customer/en/portal/articles/2158629)
* [Javascript configuration variables](https://help.disqus.com/customer/en/portal/articles/472098-javascript-configuration-variables)

