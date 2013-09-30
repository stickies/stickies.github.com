---
layout: stickie
title: Drb For President
meta: ruby api json http
---
I'm a bit tired of people trying to tell me what an api is. No it's not a json endpoint, go look it up.

I'm also really tired of people telling me that Ruby's Drb 'isnt stable' or 'isnt mature' enough to use for backend api work, a comment that I have heard on more than one occasion, used to deflect the argument because the individual doesn't know quite what it is yet, and you caught them off guard. It's been in the ruby standard library since ruby 1.9.3 for Christs sake, how stable do you want it to be?? ... Unlike HTTParty which is still at an *unstable* version number ... And Songkick's Oauth provider which is still at an *unstable* version number.

The first tech company I worked for used a SOAP api over a .NET service for their *public consumers*, and this worked fine, it was horrible and I wouldn't wish it on anyone trying to develop a backend api. So lets get one thing straight, when it comes to api's I hate http full stop, its stateless, over complicated to imlement, has stupid idiosyncracies dating back to the previous century and is *extremely brittle* as a transfer medium. As far as I'm concerned you can take your HTTPartly gem and shove it up your Oauth for all I care when it comes to doing backend api's.

Me: Whats wrong with using SQL over SSH if we need to extract services to another location.

Person: It's not future proof.

Me: It's more future proof than going out of business trying to exchange all your internal data over brittle, restrictive, json api's that'll need five gems to work and two weeks implementing oauth. It is at least a step towards an api and it doesn't cost you 50 grand to build.

### I mean, HTTP and JSON?
OK HTTP and JSON are without doubt *the best choice* for developing an api that will be consumed by *THE PUBLIC*. But you are a true klutz if you plan on using this as a technology choice or the backend.

I'm sorry but who in their right mind would write a backend service api over HTTP??? I'll tell you who, a company with 150 developers in twelve countries and 10 years in the pipe. If you're a start up operating in one or maybe two countries, this is, in my mind an exremely poor decision and total overkill.

## Theres more than one way to skin a large ferocious tiger thats bearing down on you in the open.
If I'm writing an internal api to allow two internal services to talk to each other using http and json, I have to do something like the following:

### API SERVER

    gem 'httparty' # 20 thousand lines of code
    gem 'songkick-oauth-provider' # 20 thousand lines of code
    gem 'rabl' # 20 thousand lines of code
    gem 'grape' # 20 thousand lines of code
    gem 'draper' # 20 thousand lines of code

### CLIENT APP

    gem 'oauth-client'

If I write an internal api that uses SQL over ssh I have to do the following:

### API SERVER

    # NO LINES OF CODE

    # I USE SSH!

If I write an internal api using DRB I have to do the following:

### API SERVER

    # NO LINES OF CODE

    I WRITE MY OWN AUTH TOKEN IN 5 LINES OF CODE!

Oh but what if I want to write my client in noddingofftosleep.js .... I DON'T CARE, YOU CHOSE TO WRITE IT IN RUBY TO START WITH SO WTF WOULD YOU SUDDENLY CHOOSE NODE OR SOME OTHER STACK ... OTHER THAN TO BE A COMPLETE ASSHOLE WHO LIKES TO CAUSE UNBOUNDED PAIN TO THE BUSINESS AND OTHER DEVELOPERS.
