---
layout: stickie
title: Drb For President
meta: ruby api json http druby drb
---

The first tech company I worked for used a SOAP api over a .NET service for their *public consumers*, and this worked fine, it was horrible and I wouldn't wish it on anyone trying to develop a backend api. So as far as I'm concerned, when it comes to backend api development avoid http like the plague, its stateless, over complicated to implement, has many idiosyncracies dating back to the previous century and is, in my opionion *quite brittle* for building API's.

### HTTP and JSON
HTTP and JSON are without doubt *great choices* for developing an api that will be consumed by *THE PUBLIC OR YOUR CUSTOMERS*. But you are on to a fail if you plan on using this as a technology choice for the backend, because you'll likely end up with an extremely complex mesh of end points interwoven between different services trying to talk to each other with a versioning nightmare in the post. You remember DLL hell?? Welcome to Fry by API.

When is it a good idea? If you're a company with 150 developers in twelve countries and 10 years in the pipe. If you're a start up operating in one or maybe two countries, this approach is, in my mind, a poor decision and also overkill.

## Theres more than one way to skin a cat.

#### If I'm writing an internal api in ruby to allow two internal services to talk to each other using http and json, I have to do something like the following:

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

    + An impossibly difficult setup, if you ever want to use me in UAT.

    ... Now implement consuming the API # 200 lines of code!

... and this is just for one endpoint!

#### If I write an internal api that uses SQL ( ActiveRecord ) over ssh I only have to do the following:

### API SERVER

    Gemfile
    # NO LINES OF CODE

    Implement my active record objects in a shared library or gem.

### CLIENT APP

    # I USE SSH!
    Use the shared active record objects from the shared gem, and make my nice secure sql connection over SSH, no heavy or over complicated HTTP headers or authentication, plus I have access to an exremely powerful api ( SQL and Active Record ).

So for some strange reason SQL over TCP isn't good enough, fine, or maybe empowering the programmer is not your objective, so:

#### [If I write an internal api using DRB I only have to do the following:](https://github.com/stevemartin/api-over-druby)

### API SERVER

    Gemfile
    # NO LINES OF CODE

    I WRITE MY OWN AUTH SERVICE IN 5 LINES OF CODE!
    DRb exposes *exactly* the hashes or objects that I want over its own protocol with no stupid headers, no http idiosyncracies, STATE, no httparting, no complex oauth and no layers of controllers and templates.
    I don't need layers of HTTP endpoints ( just ruby methods on api objects ), I don't need some complex auth, I just write my PORO api endpoint NO FUSS!

### CLIENT APP

    I WRITE MY OWN AUTH CLIENT IN 5 LINES OF CODE! OR, I JUST RUN IT OVER SSH OR SSL!

