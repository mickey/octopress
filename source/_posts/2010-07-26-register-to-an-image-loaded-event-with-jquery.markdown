---
layout: post
title: "Register to an image loaded event with jQuery"
date: 2010-07-26 01:29
comments: true
categories: [javascript, jquery]
---

I was recently working on a lightbox where I needed to display a remote image and re-center the lighbox once the image is loaded.
This may seems pretty simple but it took me some time to discover that jQuery does the job via the __load__ method.
As soon as the image has been loaded, the handler is called.

{% gist 2306563 %}