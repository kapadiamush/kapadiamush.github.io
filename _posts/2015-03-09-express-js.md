---
title: Understanding ExpressJS
subtitle: What's a framework bro? 
description: A quick explanation of all things ExpressJS. 
layout: post
image: /img/express-logo.png
---

### ExpressJS

> Express is a minimal and flexible Node.js web application framework that provides a robust set of features for web and mobile applications.
> With a myriad of HTTP utility methods and middleware at your disposal, creating a robust API is quick and easy.
<small>Shamelessly stole from: <a href="http://expressjs.com">ExpressJS's Homepage</a></small>

#### Explaining The Stack: Javascript, NodeJS, and ExpressJS

So if you're like me, one of the most confusing things about Javascript, NodeJS, and ExpressJS is how they all relate to one another. Unlike other frameworks and programming language relations, it seems like we're working with three different things instead of the usual two. 

Normally, when we talk about web application frameworks for various languages, we talk about two things - the interpreter and the actual web framework, like: python/flask, python/django, ruby/rails, ruby/sinatra, etc. What we tend to forget here is the actual programming langagues, instead, we focus on just the interpreter/web application framework. That's why the whole Javascript/NodeJS/ExpressJS stack looks so different. These other stacks are really: python/python/flask, python/python/django, ruby/ruby/rails, ruby/ruby/sinatra. So when we're trying to compare NodeJS to something like Ruby on Rails, we're doing it wrong. NodeJS is the equivalent to interpreter `ruby`. Javascript is the equivalent to writing the actual ruby code, and ExpressJS is what we really would like to compare Ruby on Rails against. 

So when we say our stack is: Javascript/NodeJS/ExpressJS - Javascript is our programming language, NodeJS is our interpreter, and ExpressJS is our web application framework.

<br/>

### Some Background About Web Applications

So a pretty common question (or at least I've been told its common - I've only been asked it once during an interiew) is: What happens when you type in some website address into your browser and hit 'enter'? 

If you're an asshole, you might try regurgitating some of this [shit](https://github.com/alex/what-happens-when). 

Since you're not an asshole, we get to keep things simple. What we want to cover in this part of the article/blog post is the gist of how the interent works on a higher level, so we can understand how different parts of a web application interact with each other. 

For starters, when we type in a website name into our browsers, our browsers send a `GET` request to the local DNS server to resolve what the IP address of the website is. Using the newly found IP address, it sends our `GET` request to the `PORT 80` of the web application/server in question.

When the server gets this request, it processes the request to the correct spot and executes whatever server-side code that it needs to, and then it responds.

So the flow we're looking at is: 

{% highlight text %}
Client => DNS => Application Server => Execute Our Code => Respond
{% endhighlight %}

<br/>

### Understanding the Application

So the web application we're going to start making is a simple application that allows users to have a profile and have other users comment on their profile. Each user will also get to have their own private secrets that only he or she can see. We'll build the application over the course of a few tutorials. 

In the first tutorial, we'll be creating the backend of the application as a RESTful API that'll let us make a few API calls to get a list of users and user information. For now, we'll create the web app, and go over some of its parts in detail to better understand how it all pieces together.

#### Assumptions:
* You know how to program
* You know some Javascript
* You have NodeJS (and npm) installed
* You are using a *nix device
* You know how to use the shell (terminal if you're stupid)

#### Getting Started

Let's fire up terminal and install ExpressJS. 

{% highlight bash %}
$ sudo npm install express-generator -g
{% endhighlight %}

This installs the `express` command, which allows us to create web application skeleton for us. This way, we don't have to worry about creating all the proper directories and starting app files. We'll talk more about this when we actually create the app folder.

Next, `cd` into your `Development` directory. Huh? You don't have a `Development` directory? Well make one! It's where you get to keep all your code/projects neatly organized so they're not wrecking through your `Documents` or `Desktop`. I suggest making the folder in your `Home` directory. 

Once inside your `Development` directory, let's create the app skeleton. Run: 

{% highlight bash %}
$ express fakebook
{% endhighlight %}

This should print out a bunch of stuff to the console, telling you all the various files it made. Great stuff right? Let's `cd` on in, and check it out for ourselves. Once you've `cd`'d your way into the directory, execute the following to install the packages: 

{% highlight bash %}
$ npm install
{% endhighlight %}

Also, if you're using Git, execute: 

{% highlight bash %}
$ git init 
$ echo "node_modules/*" >> .gitignore
{% endhighlight %}

#### Dissecting the <strike>Pig</strike> <em>Code</em>

Whatever your poison of Text Editior may be, go ahead and use it to open the `app.js` file. We'll start by examining this file, and then move onto understanding our first "route".

So when we open this file, at the top, we see a lot of code with this fancy thing called `require`. 

{% highlight js %}
var express = require('express');
var path = require('path');
var favicon = require('serve-favicon');
var logger = require('morgan');
var cookieParser = require('cookie-parser');
var bodyParser = require('body-parser');
{% endhighlight %}

This `require` function that we see loads up the modules. These are all various modules that we need for the basis of our application. As we build our application, we shall add to this list. If you recall from the post I wrote about [NodeJS]({% post_url 2015-03-02-node-js %}), NodeJS is primarily `asynchronous`, but here we've made an exception. The `require` function is `synchronous`. 

The more important (and more interesting) part of this file are the lines that follow: 

{% highlight js %}
var routes = require('./routes/index');
var users = require('./routes/users');
{% endhighlight %}

and the snippet:

{% highlight js %}
app.use('/', routes);
app.use('/users', users);
{% endhighlight %} 

These here are the important lines to us when it comes to actually developing our application. The prior code snippet is loading in two javascript files that are in the `routes` directory. The latter code snippet is setting up the application to use `'./routes/index.js'` file to handle any incoming requests to the `'/'` route, and to use `'./routes/users.js'` file to handle any incoming requests to the `'/users'` route. 

What does that even mean? 

It's definitely more confusing when we talk about the root route. It's better explained by the latter router: 

If we were trying to respond to a `GET` request posted at `localhost:3000/users/`, our application would direct us over to the router in charge of `'/users'`(in this case, that's the file in `'./routes/users.js'`. From there, it would look for a function that responds to a  `GET` request aimed at `'/'`. Essentially, the URL of is broken down into: domain => `localhost:3000`, primary router => `/users`, secondary router => `/`. The secondary router is not required. Everything can be done in one router object. It just makes for better, modular code to seperate things out. 

##### Routes and Routers

So what are these routes and routers? 

Like I was saying earlier, routes are simply the web address where we're sending the request. Routers are the objects on the server side that help make sure that our requests are sent down the right route, so that we can get exactly what we're looking for. 

If we open up the `'./routes/index.js'` file. We'll actually see this Router object for the `'/'` route. 

`'./routes/index.js'`:
{% highlight js linenos %}
var express = require('express');
var router = express.Router();

/* GET home page. */
router.get('/', function(req, res) {
  res.render('index', { title: 'Express' });
});

module.exports = router;
{% endhighlight %}

On line 2, you can see we (rather Express) create a router object. Then on line 5, we tell it that for every GET request the comes in under `'/'`, it should execute the anonymous function we give it. 

On line 9, we tell the module to export the router object. This is the same router object we required above. 

So what we're doing with routes is basically defining the various different routes in our application and what each route does. For the most part, we'll follow a very common pattern that correlates directly to database operations. 