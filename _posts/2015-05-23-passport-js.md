---
title: Getting Users Authenticated With Your PassportJS
subtitle: No one's stealing (data) from us.
description: Moving on to authentication using PassportJS.
layout: post
image: /img/PassportJS.svg
---

### PassportJS

>Passport is authentication middleware for Node.js. Extremely flexible and modular, Passport can be unobtrusively dropped in to any Express-based web application. A comprehensive set of strategies support authentication using a username and password, Facebook, Twitter, and more. <small><em>Shamelessly stolen from [PassportJS's Homepage](http://passportjs.org).</em></small>

Passport is definitely everything it claims to be, assuming you're able to figure out how it works and why it works the way it works. It definitely took me a while to figure it out, and it wasn't until recently that I actually even got to work with non-local authentication. Still. We'll walk through this together, and we'll make sense of it all. 

<br />

#### Overview

When intergrating users into our application, there are 4 components that we need to understand: 

 * [Authentication Strategies](#authentication-stratgies) - The way we make sure our users are who they are. You've definitely seen things like "Log in with Facebook" or "Log in with Google" around the internet, and passport as these "strategies" to help you do exactly that on your site. Wait you wanna manage your own users? Well you can do that as well with what's called a "local strategy".
 * [Serialization](#serialization) - Every request that gets sent to the backend from a logged in user is going to need a way to identify each user without taking up too much space on the client side. 
 * [Session Stores](#session-stores) - Session stores are where we store what users are currently logged in. Don't worry, I'll explain how this works, but we're not going do anything with it. 
 * [Frontend Integration](#frontend-integration) - A lot of the material tells you how to deal with redirections if you're using Express for both the frontend and the backend, but we're not doing that. This part will go over about telling you how we'll identify users with BackboneJS on the frontend. 

<br />

#### Authentication Strategies

Strategies are how we process user data for registration and log in. There are two types of strategies: [Local Strategies](#local-stratgies) and [External Strategies](#external-strategies). Local strategies are basically strategies we define ourselves, and external strategies are ones thare defined by various other applications that offer OAuth, OAuth2.0, OpenID, Bearer, and [more](http://passportjs.org/docs/providers).

##### Local Strategies

For our application, we'll be defining our own local strategy. It'll help us build an entire piece of authentication, and help us understand everything before we leave it to facebook or google to do everything for us. 

Because we love modularization, we'll put our passport configurations in a folder called `config` and its own module called `passport_config.js`. 

##### Multi-User Types


##### External Strategies

<br/>

#### Serialization

##### Serializing Users

##### Deserializing Users

<br />

#### Session Stores 


<br />

#### Frontend Integration

Words.

<br />
