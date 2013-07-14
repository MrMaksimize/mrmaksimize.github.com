---
layout: post
title: "Rails 4 Strong Parameters"
description: "A quick note about Rails 4 strong parameters"
category: rails
image: http://upload.wikimedia.org/wikipedia/commons/f/f9/Surprised_young_cat.JPG
---
{% include JB/setup %}

So I've been going through the awesome [JumpStart Labs Tutorials](http://tutorials.jumpstartlab.com/) lately to learn some rails and found a small bug in the tutorial.

It's not really a bug though. The tutorial was made with Rails 3.x in mind and I've been using Rails 4.  So I found a small discrepancy and submitted an [issue](https://github.com/JumpstartLab/curriculum/issues/606)

I was strongly encouraged to blog about my journeys into Rails and Ruby so this is one of those posts.  Really hoping someone finds this useful.

<!--break-->

There's a section in the tutorials that talks about Rails protection so you can't just directly save parameters coming into the form in the controller.  This is a great thing so people can't just submit bogus data or do something worse.  In the [Form Based Workflow](http://tutorials.jumpstartlab.com/projects/blogger.html#i1:-form-based-workflow) section of the tutorial, they mention adding `attr_accessible` to be able to save the parameters coming into the form.

I tried this and got an error saying this was deprecated in Rails 4 and has been moved off to a separate gem.

After a bit of googling around, I found [a great article from RubySource](http://rubysource.com/rails-4-quick-look-strong-parameters/).

The long and short of it is this.

  * The protected parameters is a separate gem now, but is included in Rails 4
  * I added `config.active_record.whitelist_attributes = true` to config/application.rb
  * Added `include ActiveModel::ForbiddenAttributesProtection` to my Articles model.
  * Added the following method to my articles controller:

    {% highlight ruby %}
    def article_params
        params[:article].permit(:title, :body)
    end
    {% endhighlight %}

And finally, updated my Create controller accordingly:
   {% highlight ruby %}
    def create
      @article = Article.new(article_params)
      @article.save
      redirect_to article_path(@article)
    end
    {% endhighlight %}

Definitely learned something useful.  You can also use permit to allow certain users to edit certain fields if you wish as well.


