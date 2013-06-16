---
layout: stickie
title: Using Jasmine js to test prototype driven js with spies
meta: javascript, jasmine, testing, prototype
---

{{title}}
I have been working on a project recently that has invloved a lot of javascript ( as more and more projects do these days ). The approach I have seen many people, myself included, take in the past, is to write a load of functions without tests and then try and wrestle them in to scope of the project. Not this time!

We have been using [jasmine](http://pivotal.github.io/jasmine/) at work in our main app, paritally at least, we have even mixed in some coffeescript too. It can be a bit of a beast to get working in my experience, but there are a couple of gems that have helped me get it up and running in my own projects:

    gem 'jasmine-rails'
    gem 'jasmine-headless-webkit'
    gem 'guard-jasmine-headless-webkit'
    gem 'guard-rails-assets'


