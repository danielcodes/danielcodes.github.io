---
layout: post
title: Set your Disqus configuration variables 
comments: true
---

After deciding on the theme that I wanted to use for this site, [Hyde](https://github.com/poole/hyde). I was also actively looking for ways to improve the site. I found a really helpful [here](http://joshualande.com/jekyll-github-pages-poole/). This article in essence helped add an Archive, Analytics and Comments to my page. Everything was well set up, or so I thought. When setting up Disqus comments, I placed the markup provided on my page but neglected to fill in the variables that they had **strongly** suggested me to replace.

These are the variables:

```javascript
var disqus_config = function () {
	// Replace PAGE_URL with your page's canonical URL variable
	this.page.url = PAGE_URL;

	// Replace PAGE_IDENTIFIER with your page's unique identifier variable
	this.page.identifier = PAGE_IDENTIFIER;  
};
``` 

### Where I goofed up

The way Disqus works is by keying a provided URL, defined by a url variable. As you can tell, I did not define this URL and when I received the first comment on my page, sometimes it showed up and sometimes it didn't. The problem was how the page was being access, by that I mean the protocol used. Disqus was creating two different comment thread

