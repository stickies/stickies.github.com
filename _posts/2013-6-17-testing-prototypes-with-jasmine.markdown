---
layout: stickie
title: Using Jasmine to test drive prototypes
meta: javascript, jasmine, testing, prototype
---

{{title}}
I have been working on a project recently that has invloved a lot of javascript ( as more and more projects do these days ). The approach I have seen many people, myself included, take in the past, is to write a load of functions without tests and then try and wrestle them in to scope of the project. Not this time!

We have been using [jasmine](http://pivotal.github.io/jasmine/) at work in our main app, paritally at least, we have even mixed in some coffeescript too. It can be a bit of a beast to get working in my experience, but there are a couple of gems that have helped me get it up and running in my own projects:

    gem 'jasmine-rails'
    gem 'jasmine-headless-webkit'
    gem 'guard-jasmine-headless-webkit'
    gem 'guard-rails-assets'

#### The subject
I'm working on a project that involves using d3 to create maps, I want to use prototyical inheritance to make my map solution reusable and flexible. I want to test that the code works and that the maps are generated correctly.

Given I have a base prototype:

    function Map(attrs) 
      this.scale = 2000;
      this.width = 500;
      this.height = 800;
      this.center = [0, 55.4];
      this.rotate = [4.4, 0];
      this.parallels = [50, 60];
    }
    Map.prototype.setProjection = function(attrs){}

And an object that inherits properties and methods from map:

    function MiniMap(attrs){
      Map.call(this);

      this.setProjection();
    }
    MiniMap.prototype = new Map();
    lmp = MiniMap.prototype
    lmp.constructor = MiniMap;


#### The test
To test the prototype there are a couple of different approaches you can use, firstly, you can spy on individual prototype methods as seen below:

    //= require maps/application.js

    describe("MiniMap", function(){
      var mini_map = null;

      beforeEach(function() {
        spyOn(Map, 'call').andCallThrough()
        spyOn(Map.prototype, 'setProjection').andCallFake(function(){});
        mini_map = new MiniMap({id:1});
      });

      describe("build a map", function(){
        it("should have mini map attributes", function(){
          expect( mini_map.scale ).toEqual(2000)
          expect( mini_map.width ).toEqual(500)
          expect( mini_map.height ).toEqual(800)

          expect( mini_map.center ).toEqual([0, 55.4]);
          expect( mini_map.rotate ).toEqual([4.4, 0]);
          expect( mini_map.parallels ).toEqual([50, 60]);

          expect(Map.call).toHaveBeenCalled();
          expect( Map.prototype.setProjection ).toHaveBeenCalled();
        });
      });

    });

##### mock the whole prototype
Secondly, if you want, you could stub out the whole prototype with a mock/spy object like so:

    //= require maps/application.js

    describe("MiniMap", function(){
      var MapSpy, mini_map = null;

      beforeEach(function() {
        MapSpy = jasmine.createSpyObj('Map', ['setSvg',·
                                           'setProjection',·
                                           'setPath',·
                                           'build',
                                           'buildBase',
                                           'call', 'prototype']
                                  )
        MapSpy.scale = 2000;
        MapSpy.width = 500;
        MapSpy.height = 800;
        MapSpy.center = [0, 55.4]
        MapSpy.rotate = [4.4, 0]
        MapSpy.parallels = [50, 60]

        Map = MapSpy;
        MapSpy.prototype = MapSpy;
        MiniMap.prototype = MapSpy

        // spyOn(Map, 'call').andCallThrough();
        // spyOn(Map.prototype, 'setProjection').andCallThrough();
        // spyOn(Map.prototype, 'setPath').andCallThrough();
        // spyOn(Map.prototype, 'setSvg').andCallThrough();
        mini_map = new MiniMap({id:1});
      });

      describe("build a map", function(){
        it("should have mini map attributes", function(){
          expect( mini_map.scale ).toEqual(2000)
          expect( mini_map.width ).toEqual(500)
          expect( mini_map.height ).toEqual(800)

          expect( mini_map.center ).toEqual([0, 55.4]);
          expect( mini_map.rotate ).toEqual([4.4, 0]);
          expect( mini_map.parallels ).toEqual([50, 60]);

          expect(Map.call).toHaveBeenCalled();
          expect( Map.prototype.setProjection ).toHaveBeenCalled();
          expect( Map.prototype.setPath ).toHaveBeenCalled();
          expect( Map.prototype.setSvg ).toHaveBeenCalledWith('mini_map');
        });
      });

    });
