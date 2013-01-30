---
layout: post
title: "Optional local in partial that doesn't break"
date: 2010-05-22 00:58
comments: true
categories: [rails]
---

If you have a partial called with or without local parameters, example :

{% gist 2306385 _partial.haml %}

And you don’t want the partial to break when the local isn’t assigned, you can access it through the __local_assigns__ variable :

{% gist 2306402 _partial.haml %}