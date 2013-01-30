---
layout: post
title: "Creating an activity feed with rails"
date: 2010-05-23 01:12
comments: true
categories: [rails]
---

I recently had to build an activity feed at [letitcast.com](https://letitcast.com).
It’s a common thing to do, but I thought I might mention how I did it, and see if anyone else has suggestions/improvements.

We wanted to display to our actors new roles corresponding their profile, new feedbacks and views of their auditions, recent comments ...

So, I first created a model **Activity**, here's the migration :

{% gist 2306479 %}

As you can see it's a polymorphic model, so the activity can be related to any active record model.

Here's the model :

{% gist 2306496 %}

At the beginning the activity types were mapped in an other database but it was a bit overkill and was reducing the performance a lot so I just added them into the model (AUDITION_POSTED, AUDITION_FEEDBACK_UPDATED, ...)

Activities are created with observers.
In this exemple a new audition triggers an AUDITION_POSTED activity created.

{% gist 2306518 %}

Sooooo ... I can add any event via a simple variable in the activity model and an observer.

It was working really well but there were some performance issues with some events that required a lot of inserts in the activity model. 
I first decided to execute this observer in a delayed job but after some research I ended up with a fast way to insert a batch of activities, here's the method :

{% gist 2306541 %}

To retrieve the activities :

{% gist 2306543 %}

By default you'll get 10 activities order by most recents. 
If you want to change this behavior, just change the __default_scope__.

We're also in the process of grouping the user activities to display them in a nice view, so there will maybe be a part II :-)