---
layout: post
title:  "Breaking Production - Handling overload in the real world"
author: "Amanda Sposito"
date:   2021-01-22 21:25:00 -0300
categories:
  - elixir
  - little's law
  - performance
---

If you are lucky enough your application will evolve from an MVP with low traffic to a system that needs to handle multiple requests. There's a big difference between maintaining those two types of applications.

When our application starts to handle more traffic things start to break. An application designed to handle [5.000 requests a day](https://blog.twitter.com/official/en_us/a/2010/measuring-tweets.html), it's not the same that can handle [500 million requests a day](https://blog.twitter.com/official/en_us/a/2014/the-2014-yearontwitter.html). Most of us should remember [Twitter's Fail Whale](https://thenextweb.com/twitter/2013/11/25/rip-fail-whale/) and the struggle to keep pace with its users and the sheer volume of tweets.

So, what should we do?

### Observability

First of all, you need to understand **what** your application is doing.


### Planning for high loads

"Just turn on auto-scaling on AWS" they say.


### Little's Law

### Back-pressure vs Load shedding

### Surviving spikes with workload distribution

