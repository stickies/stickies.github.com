---
layout: stickie
title: Drb For President
meta: ruby api json http
---
I'm a bit tired of people trying to tell me what an api is. No it's not a json endpoint, go look it up.

I'm also really tired of people telling me that Ruby's Drb 'isnt stable' or 'isnt mature' enough to use for backend api work, a comment that I have heard on more than one occasion, used to deflect the argument because the individual doesn't know quite what it is yet, and you caught them off guard. It's been in the ruby standard library since ruby 1.9.3 for Christs sake, how stable do you want it to be?? ... Unlike HTTParty which is still at an *unstable* version number ... And Songkick's Oauth provider which is still at an *unstable* version number.

The first tech company I worked for used a SOAP api over a .NET service for their *public consumers*, and this worked fine, it was horrible and I wouldn't wish it on anyone trying to develop a backend api. So lets get one thing straight, when it comes to api's I hate http full stop, its stateless, over complicated to imlement, has stupid idiosyncracies dating back to the previous century and is *extremely brittle* as a transfer medium. As far as I'm concerned you can take your HTTPartly gem and shove it up your Oauth for all I care when it comes to doing backend api's.

I'm sorry but who in their right mind would write a backend service api over HTTP??? I'll tell you who, a company with 150 developers in twelve countries and 10 years in the pipe. If you're a start up operating in one or maybe two countries, this is, in my mind an exremely poor decision and total overkill.

Me: Whats wrong with using SQL over SSH if we need to extract services to another location.

Person: It's not future proof.

Me: It's more future proof than going out of business trying to exchange all your internal data over brittle, restrictive, json api's that'll need five gems to work and two weeks implementing oauth. It is at least a step towards an api and it doesn't cost you 50 grand to build.

## Theres more than one way to skin a large ferocious tiger thats bearing down on you in the open.
If I'm writing an internal api to allow two internal services to talk to each other using http, I have to do something like the following:


If I write an internal api that uses SQL over ssh I have to do the following.

If I write an internal api using DRB I have to do the following.

Oh but what if I want to write my client in noddingofftosleep.js .... I DON'T CARE, YOU CHOSE TO WRITE IT IN RUBY TO START WITH SO WTF WOULD YOU SUDDENLY CHOOSE NODE?
