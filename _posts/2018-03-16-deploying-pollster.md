---
layout: post
title: Creating a Voting App with create-react-app and Django
comments: true
---

The latest project that I worked on was **Pollster**, this is the Voting App in Free Code Camp's curriculumn. This was a good chance for me to build a project from the ground up and play with technologies that I had been eye balling for a while (mostly Redux). This also meant deploying the application. Check Github repo [here](https://github.com/danielcodes/voting_app).

Here are a few pictures:

<ul class="bxslider">
	<li><img src="/public/img/pollster/pollster_1.png" alt=""></li>
	<li><img src="/public/img/pollster/pollster_2.png" alt=""></li>
	<li><img src="/public/img/pollster/pollster_3.png" alt=""></li>
</ul>

The stack that I chose to use was:

* **React + Redux**
* **Django** (especifically **Django Rest Framework**)

I had used React for several side projects but had yet to integrate Redux for state management. This was a good place to start. After reading through the official docs and several tutorials, I settled on using the model provided in [this tutorial](http://jasonwatmore.com/post/2017/12/07/react-redux-jwt-authentication-tutorial-example). However, after getting started I soon ran into trouble as adding CSS required modifying the webpack configuration. Also, I would need to configure bundling the app to be production ready. Due to this, I migrated most of the app to [create-react-app](https://github.com/facebook/create-react-app). Using CRA, I saved myself some configuration steps and took the good bits from the other tutorial.

From here on, developing the application took the following workflow:

* Create a page and decide on what was the data needed
* Create the **constants** for the actions
* Define the **actions** that would call the services, along with request, success and failure functions
* Define **services**, which make the actual http calls to retrieve data handled in the actions
* Define the **reducers** that will update the **new state** accordingly

This was actually a pretty tedious process. I guess that's the Redux way of things. Also, if that made zero sense, take a look at the tutorial listed above.

Before starting any work on the above, I had hastily set up an API using **Django Rest Framework** using viewsets and routers. By creating a few models and their serializers, you can have a full CRUD API with very few lines of code.

With this configuration, I had the following routes:

* **/questions**
* **/questions/_id**
* **/choices**
* **/choices/_id**
* **/questions/_id/choices**
* **/questions/user/user_id**

The first four were given for free, the last two I had to define on my own. Besides this, I used the `djoser` package to provide **JWT authentication**. Stitching this API and my frontend, I had put together a barebones application.

However, for certain user stories, I needed to secure parts of the application (ie. only auth users can create polls, only owners can delete questions, etc). For this, I needed to add certain permissions to the application and having the viewsets abstracting a lot of the `HTTP methods` for each route, I didn't have the finer control that I needed. I ended up having to rewrite the routes. I used this chance to write a full test suite for each of the routes, following [this guide](https://realpython.com/blog/python/test-driven-development-of-a-django-restful-api/).

### Requests in development

While developing the app, CRA has this `proxy` key that you can throw in `package.json`. This basically tells webpack to proxy requests to the specified port. For my Django API being served on port `8000`, the setting looked like this:

~~~
'proxy': 'http://localhost:8000'
~~~

### Preparing for production

I had worked on the app for a week and a half before I had completed all the [user stories](https://www.freecodecamp.org/challenges/build-a-voting-app). Naturally the next step was deploying the application. Here is where I was stumped for some time. I asked, **How would I mimic this development set up in production?**

I proceded by first creating the build bundle with `yarn build`, then to serve the static files, I ran (inside the `build` directory)

~~~
python -m http.server 3000
~~~

Here's where the app broke, requests were not going through as there was no proxy redirecting the requests. 

This followed a bit of a research period to find out how I could deploy this application. There weren't any tutorials with what I was looking for, which was deploying an SPA with a REST API, both sitting on the same machine. However, when I took a closer look, there were tutorials for doing these things separately. I found the following two:

* [Deploying create-react-app with Nginx and Ubuntu](https://medium.com/@timmykko/deploying-create-react-app-with-nginx-and-ubuntu-e6fe83c5e9e7)
* [How to Deploy a Django Application to Digital Ocean](https://simpleisbetterthancomplex.com/tutorial/2016/10/14/how-to-deploy-to-digital-ocean.html)

The key to deploying the application was mixing these two tutorials together. Have Nginx serve the static files from the `/build` directory created by `yarn build` and proxy requests to the port where the Django application was being served. There was only one thing left to do, try it.

### Taking action

I started by first deploying my Django REST API as that was the tougher of the two. I started by first provisioning a $5 linux box from Linode (had some leftover credits). I followed these two guides to set up my server:

* [Getting started with Linode](https://linode.com/docs/getting-started/)
* [How to secure your server](https://linode.com/docs/security/securing-your-server/)

Once this was done, I followed the tutorial for deploying a Django app. The steps looked like this:

* Update the machine (`apt-get update and upgrade`)
* Install system dependencies (**PostgreSQL**, **Nginx**, **Supervisor**, **Python packages**, etc)
* Create the **python virtualenv** and install the app dependencies
* Configure **Gunicorn** to serve the application
* Configure **Supervisor** to run the Gunicorn server 
* Configure **Nginx** to proxy requests to Gunicorn

### Adding in the frontend

Once the Django application was running, now I needed to serve the static files from the `build` directory. Changes were made to the [original Nginx configuration file](https://simpleisbetterthancomplex.com/tutorial/2016/10/14/how-to-deploy-to-digital-ocean.html#configure-nginx).

To serve the static files from `build`, these two lines were added

~~~
root /home/daniel/voting_app/client/build;
index index.html index.htm;
~~~

Another change was that now the `/` route would serve `index.html` not proxy to the Django app

~~~
location / {
  try_files $uri /index.html;
}
~~~

Now the last change that was needed was to proxy the http calls to the Django app

~~~
location /api {
  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  proxy_set_header Host $http_host;
  proxy_redirect off;
  proxy_pass http://app_server;
}
~~~

Doing these 3 modifications gave me the behaviour I wanted, to serve the `build/` bundle created by CRA and proxy requests to Gunicorn which was serving the REST API. To see the final Nginx configuration check [this gist](https://gist.github.com/danielcodes/8e6ee3eea155d9e6bcd191e8f7bbab6d).

### Mistakes I made attempting this

This procedure was not as smooth of a transition as I described above. I am going to list some parts that I had to wrestle with before everything came together.

##### venv folder placement

While setting up Gunicorn, I initially had my `venv` directory set up two directories above my Django app directory. This caused all types of `file not found` errors when I accessed the directories with `../../venv/activate` on the Gunicorn configuration file.

To avoid this, place the `venv` only a level above your application directory. In my case this looked like:

~~~

├── client
├── project
└── venv
~~~

##### Make your API routes distinct

For the frontend, React Router takes care of the routing. In the backend, it is the configured Django urls. Not thinking ahead, I had created conflicting urls, where I had a route for `/questions` in both React Router and Django. Navigating through the site to get to `/questions` worked fine but if the url was punched in through the browser, I would end up with the Django page serving the JSON data.

To resolve this, I prefixed all Django urls with `/api`. Now the routes set up by React Router and the Django urls did not conflict.

### Conclusion

Overall, a pretty fun experience. Deployment still isn't the easiest thing to do but I'm happy with the progress.

### TL;DR

* Deploying a create-react-app SPA and a REST API in the same machine.
* Use Nginx to serve the static files from your SPA build bundle and proxy the requests to the server serving the REST API.

