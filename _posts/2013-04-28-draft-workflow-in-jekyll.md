---
layout: post
title: "Draft Publishing In Jekyll"
description: ""
category: web 
tags: 
image: http://sapac.umich.edu/files/sapac/field/image/draft.jpg
---
{% include JB/setup %}

This is a short one.  
I found a great article about how achieve draft publishing in Jekyll.  Normally, you would generate a new post, and whether it's done or not, jekyll will publish it.  However, with this you can create a bunch of stuff in the _drafts folder, then move it to the _publish directory, and the plugin will take it from the _publish directory, append the proper date to it, and move it to _posts.

[Read On Here](http://jeffreysambells.com/2013/02/01/jekyll-draft-publishing-plugin)
