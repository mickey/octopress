---
layout: post
title: "Using Nginx as a load balancer"
date: 2009-12-30 04:13
comments: true
categories: [sysadmin, nginx]
---

Here’s a look at how nginx does basic load balancing :

{% gist 2306229 nginx.conf %}

This configuration will send 50% of the requests for www.yoursite.com to yoursite1.yoursite.com and the other 50% to yoursite2.yoursite.com.

## ip_hash

You can specify the __ip_hash__ directive that guarantees the client request will always be transferred to the same server.
If this server is considered inoperative, then the request of this client will be transferred to another server.

{% gist 2306278 nginx.conf %}

## down

If one of the servers must be removed for some time, you must mark that server as *down*.

{% gist 2306287 nginx.conf %}

## weight

If you add a *weight* tag onto the end of the *server* definition you can modify the percentages of the requests send to the servers.
When there's no weight set, the weight is equal to one.

{% gist 2306307 nginx.conf %}

This configuration will send 80% of the requests to yoursite1.yoursite.com and the other 20% to yoursite2.yoursite.com.

*note:* It's not possible to combine ip_hash and weight directives.

## max_fails and fail_timeout

*max_fails* is a directive defining the number of unsuccessful attempts in the time period defined by *fail_timeout* before the server is considered inoperative. If not set, the number of attempts is one. A value of 0 turns off this check.
If *fail_timeout* is not set the time is 10 seconds.


{% gist 2306328 nginx.conf %}

In this configuration nginx will consider yoursite2.yoursite.com as inoperative if a request fails 3 times with a 30s timeout.

## backup

If the non-backup servers are all down or busy, the server(s) with the *backup* directive will be used.

{% gist 2306339 nginx.conf %}

This configuration will send 50% of the requests for www.yoursite.com to yoursite1.yoursite.com and the other 50% to yoursite2.yoursite.com.
If yoursite1.yoursite.com and yoursite2.yoursite.com both fails 3 times the requests will be send to yoursite3.yoursite.com.