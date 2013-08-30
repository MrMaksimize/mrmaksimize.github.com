---
layout: post
title: "Local Development With PhoneGap and Drupal, Rails, etc"
description: "Local dev setup for working with a PhoneGap app that connects to a locally hosted site."
category: "code"
image: "http://www.tricedesigns.com/wp-content/uploads/2013/01/Build-Bot-Preview.png"
---
{% include JB/setup %}

Let's not talk about WHY, because no one ever really knows or understands the crazy things people want to do, but I had to set up a PhoneGap project -- actually more accurately a [DrupalGap](http://drupalgap.org) project. The setup instructions for DrupalGap are not the greatest, but I think that's out of scope for this blog post.

The particular problem I was trying to solve was more PhoneGap and local development related and is rather generic. Developers running any website framework (rails, drupal, etc) will run into this if they are developing locally (as they should be since it's a best practice). I wanted to set up a local site called `http://mysite.dev`, and as I was developing the phonegap application I wanted to be able to access it on `http://app.mysite.dev`, to see what my beautiful code was crafting.

After getting everything set up, and all the files in the right places, I downloaded the Ripple Emulator (we'll get to it shortly, it's one of the steps below), but I could not get the app to connect to my drupal site.  I knew I set up the `/etc/hosts` file correctly, so I started looking at Chrome's console.  I realized that whenever the PhoneGap application made an http request to mysite.dev, Ripple would hijack the request and route it through a proxy on Heroku. That obviously wasn't working too well since `http://mysite.dev` was a local dns configuration.  I noticed there was an option to use a local proxy, but a good chunk of Googling didn't help out.  Eventually I found a [ripple emulator npm package](https://npmjs.org/package/ripple-emulator);  I installed it, and was able start the proxy.

Sooo, I figured I'd write a helpful blog post and document all the steps it takes to get set up.  Enjoy:

Make sure your /etc/hosts file points both `http://app.mysitedev` and `http://mysite.dev` to your local machine or vagrant. Here's an example of mine, but I use vagrant:

    127.0.0.1     mysite.dev
    127.0.0.1     app.mysite.dev

Also, verify your Apache / Nginx conf files.  Make sure you can get both mysite.dev and app.mysite.dev in your browser and not get a 404.

Let's get started!

* Download Ripple Emulator as a Chrome Extension from [Chrome Web Store](https://chrome.google.com/webstore/detail/ripple-emulator-beta/geelfhphabnejjhdalkjhgipohgpdnoc)
* Make sure you are logged out of the `http://mysite.dev` site.
* Visit `http://app.mysite.dev/index.html?enableripple=cordova-2.9.0`
* You will get a connection error when PhoneGap tries to make an http request to your site.
* Set The Following Settings under The Settings Box on the right side:
    * Cross Domain Proxy: Local
    * Proxy Route: `/ripple`
    * Proxy Port: `4400`
    * [Screenie](https://www.monosnap.com/image/QijT8Q0e5Z3A8FG5bnzLQqBNz)
* Refresh The Page and make sure the settings stick.
* Now, in your command line of your host machine, run `npm install -g ripple-emulator`
* If you don't have node / npm installed [this is a good resource](http://shapeshed.com/setting-up-nodejs-and-npm-on-mac-osx/)
* Run `ripple proxy --port 4400 --route /ripple`
* This will create a local ripple proxy at `localhost:4400` and will let your app communicate with your site.
* Refresh the app page and you should not be getting an error anymore.

I hope this will help someone out!!

