---
layout: post
title:  "Exploring APIs Design: Protobuf & Twirp"
author: "Amanda Sposito"
date:   2021-01-22 20:35:00 -0300
categories:
  - elixir
  - api
  - rpc
  - protobuf
---

* using RPC, Protobuf and Elixir to standardize communication between microservices, why and when to reach for RPC.

### What is RPC?

RPC stands for [Remote Procedure Call](https://en.wikipedia.org/wiki/Remote_procedure_call) and is a request-reponse protocol. It's not a new idea, [the term first appeared in 1981](http://www.bitsavers.org/pdf/xerox/parc/techReports/CSL-81-9_Remote_Procedure_Call.pdf), but the idea goes back even further than that.

The idea is that you can execute a piece of code on another server, like you would if they were on the same application. It uses a client-server model, where the application requesting is the client and the application serving the requests is the server.

The benefit of this approach is that...

### What is Protobuf?


### How can I use it in Elixir?
