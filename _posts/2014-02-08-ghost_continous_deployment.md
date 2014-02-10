---
layout: post
title: Continuous Deployment of Ghost from Github
description: Setting up automatic from GitHub for Ghost running on DigitalOcean
category: deployment
tags: 
  - deployment
  - github
  - code
  - ghost
  - digitalocean
  - server
  - hosting
comments: true
share: true
image: 
  feature: "abstract-7.jpg"
published: true
---

As part of my fellowship at [Code For America](http://codeforamerica.org), my team had to set up a blog in order to keep everyone updated about what we're doing and how everything is going.  Most teams just set up a tumblr blog.  However I had some ethical qualms about doing that and successfully was able to convince my team to agree to running [Ghost](http://tryghost.org).

Ghost is a very interesting blogging platform.  It came about as an answer to the bloat of WordPress into a massive CMS which is pretty resource intensive.  It's written entirely in Node with SQLite3 database backend.  Additionally, it provides for a very nice interface in editing posts using Markdown and basically does everything you need a simple publishing platform to do for you.

After I set it up, my team's designer, Ainsley wanted to make some CSS changes to the blog and the theme.  She would make the changes, push them up to GitHub, but the problem was she was waiting for me to jump onto the server and pull the changes in.  On top of that, every time the codebase needs an update, the Ghost service on the machine needs to be restarted.

This now meant I was a bottleneck to any development being done on the blog.  I didn't like it.

I started researching into how to automate this process, and found a few articles. Namely these two helped out:

  * [Fideloper](http://fideloper.com/node-github-autodeploy)
  * [Gun.io](http://gun.io/blog/tutorial-deploy-node-js-server-with-example/)

I would definitely recommend reading them.  However, my use case wasn't as complex as these articles describe.  However, they did point me a to pretty sweet node package called [HookShot](https://github.com/coreh/hookshot).  It's an extremely simple tool that allows you to listen for incoming GitHub post-receive hooks and react accordingly.  You can use it as a library, a CLI tool, or even mount it on top of your existing Express servers.

So, here are the steps

**I am making assumptions here such as you're running Ubuntu and have Upstart**

###Make an upstart steps for running you ghost service.

Make a file in `/etc/init/ghost-YOUR_SITE_NAME.conf`
Add the following:

{% highlight bash %}
# ghost-YOUR_SITE_NAME

start on startup

script
    cd /path/to/your/ghost/root
    npm start --production
end script
{% endhighlight %}

This will let you run the ghost process as a service.

### Write the listener script
Pick a port where you know nothing is running.  If you look at Ghost's config.js file, you will see what port the ghost service is started.  You can always run `netstat --listen` and see if the port number you chose is being used by a service.

Create a file and add it to the code base at the root of ghost;  I called it `deploy_listener.js`, but really the name doesn't matter.  I'm considering renaming it to something that has to do with monkeys, because I like monkeys.  The contents of the file are pretty simple:

{% highlight js %}
var hookshot = require('hookshot');
hookshot('refs/heads/master', 'git fetch origin && git checkout origin/master && service ghost-coqui restart').listen(PORT_NUMBER)
{% endhighlight %}

***Don't forget to replace the port number***

Now we're going to have a process listening, but putting nginx in front of this process as well didn't seem to make a lot of sense to me. The nginx would have to listen to either a subdomain or another port, and the next step would be necessary either way.  So I decided to bypass messing with nginx and opened up the firewall directly to the node server:

### Opening up the firewall for the listener

Now, we need to open the IPTABLES firewall to allow requests to the port that hookshot will be running on.  This command should do it for us:
{% highlight bash %}
# Need to be a priviledged user
$ sudo iptables -I INPUT 4 -p tcp --dport 9001 -j ACCEPT # (I)nserts this rule after the 4th iptables firewall rule
{% endhighlight %}

###Starting and running the hookshot process

Last part but not least, we need to start the hookshot process to listen for webhooks from GitHub.

I just took the much easier way and installed forever to run the listener, but I'm pretty sure you can just set it up as another upstart process.  In fact, you probably should since that would allow it to start running whenever your server gets rebooted. If you want to do that:

Make a file in `/etc/init/ghost-YOUR_SITE_NAME-listener.conf`
Add the following:

{% highlight bash %}
# ghost-YOUR_SITE_NAME-listener

start on startup

script
    cd /path/to/your/ghost/root
    node deploy_listener.js
end script
{% endhighlight %}

The other option is use forever to run the process.

{% highlight bash %}
npm install -g forever
forever start /path/to/your/ghost/deploy_listener.js
{% endhighlight %}

### Getting github's webhook set up

* Go To Settings on your GitHub repo: ![Github Settings](https://monosnap.com/image/wLflXqInHVFsMsMBMMsaZoelSSeHVr.png)
* In the left menu, select Service Hooks.
* Under services click on Webhook URLs and add the url of your path with the port number after it. For example for our blog it's `coquicoders.org:9732`
* After you save, you should have a button to test the hook ![Screenshot](https://monosnap.com/image/uNqdDjBeq4jwFVedg9Chk3Dw211UGf.png)
* Test the hook and make sure it works.  It might help for testing if you just run `node deploy_helper.js` before you start it up as a forever or upstart process, just so you can see it working.



###Things to look into
You know how I said this is quick and dirty.  I meant it.  If you have suggestions or notes, please do let me know.  Here are the things I know I need to look into:

* Should I put nginx in front of the hookshot listener?
* Permissions for restarting the ghost service?
* I don't think I need to use forever since I can just run the hookshot listener as a service.
* It may make sense to just inject hookshot directly into ghost to avoid opening up yet another port.
* Hardening the whole thing