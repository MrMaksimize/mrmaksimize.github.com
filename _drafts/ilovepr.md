---
layout: post
title: "I Love Puerto Rico"
description: ""
category:"dev"
tags: []
comments: true
share: true
image:
  background: filename.jpg
  feature: abstract-3.jpg #1024x256
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
  thumb: thumbnail-image.jpg #keep it square 200x200 px is good
---
While we were in Puerto Rico the month of February, Ainsley and myself got a little bit itchy to create something that will live on the internet.  While both of us were having a great time interviewing people, sometimes you just gotta scratch that itch.

Ainsley found a site designed for people to show appreciation for designers [We Heart Designers](http://weheartdesigners.com).  I checked it out, and it was a WordPress site.  I have nothing against WordPress, but I didn't want to use a full blown CMS for this usecase, and the code for the site is no open source.

So, we decided to take another approach -- Node.

I picked up a project on Github called [Hackathon Starter](https://github.com/sahat/hackathon-starter) which I cannot recommend enough.  It's a very well documented boilerplate / starter kit for a quick project just like this. It's using [Express](http://expressjs.com)  for the web framework, [MongoDB](http://mongodb.com) for the database, and quite a few other various and [useful packages](https://github.com/sahat/hackathon-starter#list-of-packages).

For the front end, it uses the [Jade](http://jade-lang.com) templating language.  In addition, we used [JQuery Isotope](http://isotope.metafizzy.co/) to create all those sleek responsive animations you see when blocks are moving around.

The project has been deployed on Heroku.  The only thing I had to install that was not already included in the hackathon starter was a node package called [dotenv](https://github.com/scottmotte/dotenv).  It let me maintain a local file with some of the configuration settiongs that I would be setting up in Heroku config variables.

I set up 3 paths in express:
*  [GET] `/` -- the main path that renders the barebones pages for all the notes.  The view that this calls does not contain any notes yet since they are loaded in via ajax.
*  [POST] `/note/new` -- This is the path that the form data from the note creation form gets posted to.
*  [GET]  `/notes/:skip/:limit` -- A path that returns notes stored in the database based on a skip and limit parameter.  This is what's used to load notes into the page, and also used to load additional notes when the user scrolls to the bottom of the page.

I actually had a really cool thing going too at one point where I wrote all these interactions using socket.io.  That way when a user was looking at the page, and a new note was added by someone else, she would instantly get the notification.

This could've scaled well, but it kept falling over for me on Heroku.  Of course, I blamed it on sockets, since this is really the first time I used the technology.  So I rewrote the app to use standard ajax and it still kept falling over.  After debugging for a little while, I realized the mistake I was making.

I was not explicitly setting `NODE_ENV = 'production'` and therefore the bundled connect-assets (which is a node library designed to work similarly to the asset pipeline in Rails) was compiling LESS and minifying javascript every time the page was loaded, just like during development.

As soon as I set the variable, my load testing started to work and the app could handle much more traffic on a small amount of dynos.

The app has gotten forked by several of the other fellowship teams.  It currently lives online at [I Love Puerto Rico](http://www.ilovepuertorico.org), and the codebase can be found on [GitHub](https://github.com/CoquiCoders/ilovepr).

As always, questions / comments / patches welcome!
