---
title: Why Mongooses Are Your Best Friend
subtitle: Seriously... Who needs dogs or diamonds? 
description: Web Applications aren't complete without persistant storage. You'll need to keep track of all your data, so that you can continually restart and update your application without losing all of your data.
layout: post
image: /img/mongoose.png
---

### MongooseJS

>Mongoose is a Node.js library that provides MongoDB object mapping similar to ORM with a familiar interface within Node.js. If you aren't familiar with Object Relational Mapping (ORM) or, Object Data Mapping (ODM) in the case of Mongoose, this means is that Mongoose translates data in the database to JavaScript objects for use in your application.
<small><em>Shamlessly stolen from: [Modulus's Tutorial Blog Post About Mongoose](http://blog.modulus.io/getting-started-with-mongoose)</em></small>

Mongoose is basicallly a wrapper for using MongoDB. It's nice because you can define a schema for an object (which I'm pretty sure invalidates the whole point of MongoDB, but it does makes our lives easier). Mongoose allows us to create or update javascript/database objects, and save them with a simple `.save()` function. RESTful APIs are a breeze with MongooseJS.

<br />

### Why Databases Doe?

No web application is complete without persistent storage. There are two different ways of keep all the data from your web application stored: saving everything on the hard drive as a file or some sort (serialized object, write everything to a text file, etc.) or storing everything in a database. 

There are reasons for choosing the prior, but they are usually specific reasons like: saving serialized objects, running a file upload/download server, allowing users to add content such as images, videos, pdfs, etc. and more. 

But when it actually comes down to storing user information and other things that can be modelled easily, we often go with a database. Databases offer a consistent, fast 

[Here is a great list of reasons of why you should pick a database over storing files on a hard disk](http://arstechnica.com/information-technology/2013/05/why-use-a-database-instead-of-just-saving-your-data-to-disk/)

#### Why We're Using MongoDB (A NoSQL Database)
When I wrote my first NodeJS application, I tried using a PostgreSQL database. It didn't work, and I was getting a lot of errors. I couldn't get it to work. I was scarred. Since I was 21 and already had more than my fair share of gray hair, I decided to give MongoDB a try. When I used it for the first time - it worked flawlessly. Since then, I've just been too scarred (and too protective of my remaining black hair) to give SQL a second chance. 

That being said, I'll definitely be giving SQL databases another shot soon. I find they're really good when it comes to doing JOINs, and that there is a quiet simplicity about primary keys and foreign keys that just makes everything as soothing as a hot tea with honey when you're home alone on a rainy day, sick with a sore throat. 


#### How the App Will Act
While ExpressJS might trick you into thinking that it's okay to have your templates/frontend all in one gigantic application, it's the equilvalent of [Hitler establishing the Gestapo](http://www.ushmm.org/wlc/en/article.php?ModuleId=10005686).

What we want the backend of our application (the RESTful API portion) to do is return us JSONs that represent that data we asked for. The frontend application will make calls to our backend API, and then, it will display the data appropriately. The frontend application will make a http request to the backend; the backend application then will do one of four things:

* go into the database and grab whatever information we need
* create a database model object and save it (creating a user, a post, etc.)
* update an existing database model object (updating a user, a post, etc.)
* delete an existing database model object (delete a user, a post, etc)

These four actions directly correlate to CRUD database action.

##### From GPPD to CRUD

<b> GET is READ: </b>

`GET` requests correspond to `READ` requests in the database. Usually the "caller" is requesting some information, such as details about a specific user or a list of users. `GET` requests are never used to log a user in or create a user's account. They specifically used only for getting information from the application's database.

An example of a `GET` request code that would be in our application: 

{% highlight js %}
var User = require('../models/user');

router.get('/users', function(req, res){
    User.find(function(err, users){
        res.json({
            'status' : true, 
            'users' : users
        });
    });
});
{% endhighlight %} 
<small><b> This is just an excerpt - there is no error checking or any safety.</b></small>

On the frontend side of the application we would so something along the lines of: 

{% highlight js%}

var request =  new XMLHttpRequest(); 
request.open('GET', 'api.application.com/users', true);

//the callback function
request.onreadystatechange = function(){
    var userList = JSON.parse(request.reponseText); 

    var table = document.getElementById('userTable'); 
    /* 
     * fill in table with user information
     */
}

request.send(); 

{% endhighlight %}

<small><b> This is a vague generic excerpt, if and when we use an actual javascript frontend application framework we wouldn't be doing it like this</b></small>


<b> POST is CREATE: </b>

`POST` requests correspond to `CREATE` requests in the database. This is when we're trying to create a user or some other model. 

An example of a post request handler on the server side would be: 

{% highlight js %}
var User = require('../models/user');

router.post('/users', function(req res){
    var user = new User(); 
    /* 
     *  Fill in various user attributes 
     */ 

    user.save(function(err){
        res.json({
            'status' : true,
            'message' : "User created successfully."
        });
    });
}
{% endhighlight %}
<small><b> This is just an excerpt - there is no error checking or any safety.</b></small>

And then on the frontend we would create a post request with user's information stored in the body of the request. And since none of us really want to buy an SSL certifiate to make the website `https`. We'll send it through the internet in plain text cause we're assholes. 

The corresponding frontend code might look something like this: 
{% highlight js %}

var user = {
    "username" : document.getElementById('username'),
    "password" : document.getElementById('password')
}

var request = new XMLHttpRequest();
request.open('POST', 'api.application.com/users', true);
request.setHeader('Content-type', 'application/json');

request.onreadystatechange(function(){
    var response = JSON.parse(request.responseText); 
    // do what you want with the response
});

request.send(JSON.stringify(user));

{% endhighlight %}

<b> The Rest: PUT and DELETE </b>

`PUT` requests correspond to `UPDATE` operations in the database. You would write the code similarly to `POST` requests, execept in the database update code you would run something like: 

{% highlight js %}

router.put('/users/:username', function(req res){
    //...

    User.update({'username' : req.params.username}, { $set : { /* object with new values */ }}, function(err, results){
        //results contains how many things were updated and more...
    });

    //... 
});

{% endhighlight%}


`DELETE` request correspond to `DESTROY` operations in the database. Simply, we're just deleting things out of our database. 

<br/>

### Okay FINE. Databases are cool.

So databases are the best way to store our user related information. MongooseJS is a neat little overlay above MongoDB, so that we don't have to manually access MongoDB and have it do what we need. MongooseJS provides a library of objects and methods that take care of writing to and reading from the database. 

There's a lot of great documentation for MongooseJS, so I'm not going to spend time documenting it out. 

#### What It Do 

When using Mongoose (and really with all programming), make sure you keep everything modular. Instead of throwing a couple models into one Javascript file called "models", and then grabbing whatevery you need from the exported object, it's always better to create a models folder and keep you models in separate folders.  

