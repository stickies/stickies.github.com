---
layout: stickie
title: Drb For President
meta: ruby api json http druby drb
---
I'm a bit tired of people trying to tell me what an api is. No it's not a json endpoint, go look it up.

I'm also quite tired of people telling me that Ruby's Drb 'isnt stable' or 'isnt mature' enough to use for backend api work, a comment that I have heard on more than one occasion, even though it existed since before Rails and it's been in the [ruby standard library](http://www.ruby-doc.org/stdlib-1.9.3/libdoc/drb/rdoc/DRb.html) since ruby 1.9.3, how stable do you want it to be?? ... Unlike HTTParty which is still at an *unstable* version number ... And Songkick's Oauth provider which is still at an *unstable* version number.

The first tech company I worked for used a SOAP api over a .NET service for their *public consumers*, and this worked fine, it was horrible and I wouldn't wish it on anyone trying to develop a backend api. So lets get one thing straight, when it comes to api's I hate http full stop, its stateless, over complicated to implement, has stupid idiosyncracies dating back to the previous century and is *extremely brittle* for building API's. As far as I'm concerned you can take your HTTPartly gem and shove it up your Oauth for all I care when it comes to doing backend api's.

### I mean, HTTP and JSON?
OK HTTP and JSON are without doubt *the best choice* for developing an api that will be consumed by *THE PUBLIC*. But you are on to a fail if you plan on using this as a technology choice for the backend, because you'll end up with an extremely complex mesh of end points interwoven between different services trying to talk to each other with a versioning nightmare in the post. You remember DLL hell?? Welcome to Fry by API.

I'm sorry but who in their right mind would write a backend service api over HTTP??? I'll tell you who, a company with 150 developers in twelve countries and 10 years in the pipe. If you're a start up operating in one or maybe two countries, this is, in my mind an exremely poor decision and total overkill.

## Theres more than one way to skin a large ferocious tiger thats bearing down on you in the open.

#### If I'm writing an internal api to allow two internal services to talk to each other using http and json, I have to do something like the following:

### API SERVER

    Gemfile
    gem 'songkick-oauth-provider' # 20 thousand lines of code!
    gem 'rabl' # 20 thousand lines of code!
    gem 'grape' # 20 thousand lines of code!
    gem 'draper' # 20 thousand lines of code!

    Now implement the OAUTH # 200 lines of code!
    Now implement the API endpoint # 200 lines of code!


### CLIENT APP

    Gemfile
    gem 'oauth-client' # 20 thousand lines of code!
    gem 'httparty' # 20 thousand lines of code!

    + An impossibly difficult set up if you ever want to use me in UAT.

    ... Now implement consuming the API # 200 lines of code!

... and this is just for one endpoint!

#### If I write an internal api that uses SQL over ssh I only have to do the following:

### API SERVER

    Gemfile
    # NO LINES OF CODE

    Implement my active record objects in a shared library or gem.

### CLIENT APP

    # I USE SSH!
    Use the shared active record objects from the shared gem, and make my nice secure sql connection over SSH, no ridiculously over complicated HTTP headers or authentication, plus I have access to an exremely powerful api.

**Me:** Whats wrong with using SQL over SSH if we need to extract services to another location.

**Person:** It's not future proof.

**Me:** It's more future proof than going out of business trying to exchange all your internal data over brittle, restrictive, json api's that'll need five gems to work and two weeks implementing oauth, let alone the months of actually impementing all the endpoints. It is at least a *step* towards an api and it doesn't cost you 50 grand to build.

So for some strange reason SQL over TCP isn't good enough, fine, or maybe empowering the programmer is not your objective, so:

#### [If I write an internal api using DRB I only have to do the following:](https://github.com/stevemartin/api-over-druby)

### API SERVER

    Gemfile
    # NO LINES OF CODE

    I WRITE MY OWN AUTH SERVICE IN 5 LINES OF CODE!
    DRb exposes *exactly* the hashes or objects that I want over its own protocol with no stupid headers, no http idiosyncracies, STATE, no httparting, no stupid oauth and no stupid layers of controllers and templates.

### CLIENT APP

    I WRITE MY OWN AUTH SUBSCRIBER IN 5 LINES OF CODE!

Oh but what if I want to write my client in noddingofftosleep.js .... I DON'T CARE, YOU CHOSE TO WRITE IT IN RUBY TO START WITH SO WTF WOULD YOU SUDDENLY CHOOSE NODE OR SOME OTHER STACK ... OTHER THAN TO BE A PASSIVE AGGRESSIVE PSYCHOPATH WHO LIKES TO CAUSE UNBOUNDED PAIN TO THE BUSINESS AND OTHER DEVELOPERS.
