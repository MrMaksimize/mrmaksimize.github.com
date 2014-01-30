---
layout: post
title: "The Zen Of Code"
description: ""
category: code
tags: [code]
comments: true
share: true
image:
  feature: abstract-10.jpg
  credit: dargadgetz
  creditlink: http://www.dargadgetz.com/ios-7-abstract-wallpaper-pack-for-iphone-5-and-ipod-touch-retina/
---


Sometimes you just need to get it out the door. Write fast, make it work, roll it out, accrue technical debt, deal with it later.

But is it worth it? Lately I've been pondering a lot about the zen that comes with good code: how it makes one feel as a developer, and what does it buy?

Take a module that you've hammered out over the past three weeks. It's a quick fix for something that needs to happen on a deadline, and you just didn't have time to write comments, conform to coding standards, and look for functions to reuse.

The days are getting shorter and the problems are getting smaller, until you're finally done! Great! You've made it work with a messy patchwork of if statements, for loops and undocumented functions. The deadline hits. But wait. The requirements have changed just a bit. The change is small, but because you haven't taken the time to think about how you structured your objects, it'll require a massive rewrite.  Bummer.

You get back to drawing board and try to figure out what get_mapped_tree_nodes($id, $level, $active, &$tree) does.  Think about it for a while and can't remember. Man should've documented this. Let's debug. A few repeats of this later, angry and confused you finally finish and complete the requirements. Great! You're done!


Submit your pull request, get your peers to review. Approve Approve! Let's get this in!  After the merge, you guys realize that you just crashed your production environment because bad coding standards made your reviewers miss a major bug. Try to see what caused it. Which commit was this? Was it `Add pf3 to ln42` or `Add pf2 to gs43`? Nuff said.

<!--break-->

A month later another project requires very similar functionality, but because you failed to properly abstract your functions, you can't just pull them out or reuse them. On top of that, you don't know what half of them do, and the datasets you used for testing before are gone. Sigh. Another 3 weeks of sleepless nights. Let's write more code.

Don't tell me this never happened to you. I won't tell you it's never happened to me. And you know what? I guarantee it'll happen to both of us time and time again. So how do we STOP?

I've been thinking about this for a while, and I have come to believe there is a certain zen that comes with writing good code. Just like good deeds beget good deeds, bad code begets bad code, and good code begets good code. It's like a windfall. You can't start off messy and all of a sudden switch to clean and structured, and you can't do it in reverse.

Well-written code takes time. It requires care, attention and love. It needs nurturing and refactoring. Worst of all, it craves extra time--time that you may not have to give. But just like a child caring for their parent in their sunset years, good code will be sure to take care of you. It'll be there when you need it for reuse on your next project, and it'll be there when the requirements change. You can't say that about the mumbo jumbo you just wrote to fulfill the latest requirement.

There is no bullet list to follow in this post because every language has its proper structure and usage. No guidelines, no rules. But there is a mindset. Don't be afraid to go back and refractor your functions for the better. Don't be scared of breaking that 5000 line file into 10 smaller ones if it makes sense. And take the damn time to make your commit messages make some sense. Oh and write some docs!

I guarantee, when you make that pull request, or when you come back to to reuse your code on the next project, you'll feel much happier. Much calmer. Much more complete.

Ready? Take a deep breath. Get a beer. Get a coffee. Mix them together and chug. Now you're in a perfect state to write good code. Give it some love!

(Image from [xkcd](http://xkcd.com/844/))
