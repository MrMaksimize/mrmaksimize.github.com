---
layout: post
title: ""
description: ""
category:
image: ""
---
This may be the most obvious thing in the world to most people, but I ran into this issue so I figured I should share it, for any chumps out there.  Here's the deal.

I was using the awesome [FPPopoverController](https://github.com/50pixels/FPPopover) by 50Pixels, and I had this issue.  When I would first allocate and initalize the popover controller, it would work fine and show up with its content view, but when I clicked outside of it in an attempt to dismiss it, my app would crash.

I was getting a message telling me that a message was being sent to a deallocated instance.  So I start thinking - well how the heck does it deallocated right?  Well it makes perfect sense -- ARC is taking care of this for me.  So I'm now in a situation where I have to try to keep this controller around until I actually want it to be deallocated.  Anyhoo, here's what happened.

<script src="https://gist.github.com/MrMaksimize/6259395.js"></script>

As you can see - my solytion was to give the controller its own instance variable and have it sit around until I was ready to kill it off, not ARC.

That's it! Toodle oo!
