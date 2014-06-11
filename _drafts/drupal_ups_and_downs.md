---
layout: post
title: ""
description: ""
category:
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
Hi Giancarlo,

So as you asked, I'm going to give you some of my thoughts. I can do a more in-depth presentation some time this upcoming week.  I talked to Ruth and she said Wednesday may be a possibility.

### Preface
I'm mainly writing this in reference to the system as FusionWorks has proposed. The use cases are similar on a high level to how `empleosahora.pr` works.  They are planning to use MS Dynamics CRM.  I'm still very fuzzy on the minor details -- if needed it may make sense for me to talk a bit more with FusionWorks.

The high level requirements of that system as I understand them are as follows (in story form):

*  As a business owner, I would like to register my business under the law, request certain incentives, and in turn commit to creating a certain amount of jobs in a certain time frame.
*  As a business owner I would like an easy way to report my current progress on my part of the job creation commitment every several weeks.  (I actually have not heard this requirement said out loud, but I would think it should be there).
*  As an account manager (the person who works with the specific business), I would like to be able to track the progress of the businesses I work with in how far they have gotten with their job creation.
*  As an account manager or department administrator / employee I would like to be able to track contact information for various businesses currently engaged in the program.
*  As a department administrator I would like to be able to generate reports based on various metrics across all registered businesses.
*  As an IT manager, I would like to limit the costs I incur from this project.
*  As an IT manager, I would like the data to be accessible without the need for a desktop client.
*  As a person working in other departments, I would like to have access to this data.

I have worked with Drupal for a very long time and have run into the parts that make it very good and parts that make it bad for different use cases.  I will try to present this as pros and cons for Drupal targeted to the use cases of the system as I understand them, and the benefits that you can gain by using it over Microsoft Dynamics.  I want to be clear that this is a very high level view according to my understanding of the requirements and to my experience with use cases similar to this.

### Pros
* Drupal is PHP software.  This makes it accessible with a login and password from the web.  Therefore it is by default accessible to anyone with a log in.
* Drupal runs on a pretty standard LAMP stack -- no specialized server deployments are really necessary.
* There is a massive community of independent contributors and large corporations.
* There is a dedicated security team with a preset weekly release schedule of security and stability fixes as needed.
* WhiteHouse.gov runs Drupal.
* Open source.
* You would have to do extra work to block it off if you wanted to --  VPN / networking work.
* Data structures and sequential operations are extremely flexible and can be created without a lot of work. An example flow:
  *  An entity object to describe a business can be defined with properties as follow:
    *  Business Name
    *  Business Address
    *  Business Owner Name
    *  Act 22 Status
    *  Act 73 Status
    *  Jobs Created
    *  Job creation schedule (Feb 2014 - 5 Jobs, March 2014 - 5 Jobs, April 2014 - 5 Jobs, etc)
  * Based on the job schedule and jobs created, conditional actions can be taken based on time:
    ```
      if date >= March 15 && date <= March 30 && jobs_created < business.job_creation_schedule.job_target
          send_email: "Dear %user_name%", you only created %num_jobs jobs.  Please don't forget to input any new job creations"
      else if date > March 30 && jobs_created < business.job_creation_schedule.job_target
          send_email "Dear %user_name%.  We have changed your Act 73 status because you did not create enough jobs this month".
          business.act73status = 0;
    ```
    obviously this is pseudocode, but you can actually do complex task creation and management in Drupal fairly easily.
* Drupal has a very nice visual query builder system called Views, which can be learned with minimal training. It allows construction of database queries without writing any SQL.  You deal with high level Drupal entities (the business example above would be one), but are still granted the granularity of all their properties.
* Queries and reports built with Views can be saved and stored as page displays or exposed as APIs (JSON, XML, etc).
* Drupal has its own specific cron jobs which can be used to run timed operations.  This can also be done with minimal programming with a system called Rules.
* A lot of customization can be achieved with modules contributed by community, achieving faster iteration.
* CRM systems can be built with Drupal using the entity pattern I described above.  However, there are currently several CRM systems for Drupal that are actually prebuilt using the entity pattern. You still have full control of the entity properties choosing to add them or remove as you want, but it works as a great starting point for customization.  The system is called [RedHen](http://redhencrm.com/);
* Drupal if configured properly can be fairly performant.

Overall, in terms of the pros, the main strenth is being able to define and manage entity objects fairly easily, and maintaining a very high abstraction to the database. On top of that, the visual query builder and task management are extremely powerful and elegant.



###Cons
*  PHP can go here too.
*  Most of the system is designed in the sequential programming model.  There are some sections that use OOP, but for the most part it's sequential.  There is a large amount of hooks allowing for injecting your code at given points however which allows for lots of customization;
*  Dependency management is difficult, but is a problem that has been worked on and for a large part solved (at my previous employment).
*  Versioning configuration in code requires a bit of extra work. A lot of these problems have been solved however.
*  As with any other web based, database backed system, environment management is tough.  This should be getting fixed in Drupal 8 which is not stable yet, but it is in alphas. (Most likely not a candidate).  However, this has been tackled before by me and by the time I left my old job we were using a pretty streamlined development process across multiple environments and multiple developers.

Overall the cons in Drupal tend to show up during deployment, environment management, and version control.  But as mentioned above, we've previously solved a lot of these problems and I have trained people on how to implement these solutions.
