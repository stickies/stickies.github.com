---
layout: stickie
title: Using Jasmine js to test prototype driven js with spies
meta: javascript, jasmine, testing, prototype
---

{{title}}
I have been working on a project recently that has invloved a lot of javascript ( as more and more projects do these days ). The approach I have seen many people, myself included, take in the past, is to write a load of functions without tests and then try and wrestle them in to scope of the project. Not this time!

We have been using [jasmine](http\://pivotal.github.io/jasmine/) at work in our main app, paritally at least, we have even mixed in some coffeescript too. It can be a bit of a beast to get working in my experience, but there are a couple of gems that have helped me get it up and running in my own projects:

    gem 'jasmine-rails'
    gem 'jasmine-headless-webkit'
    gem 'guard-jasmine-headless-webkit'
    gem 'guard-rails-assets'

To test prototype:
    //= require maps/lenders/application.js

    describe("LenderStandardMap", function(){
      var MapSpy, d3, lender_mini_map = null;

      beforeEach(function() {
        d3 = jasmine.createSpyObj('d3', ['defer']);
        MapSpy = jasmine.createSpyObj('Map', ['setSvg',·
                                           'setProjection',·
                                           'setPath',·
                                           'build',
                                           'buildBase',
                                           'call', 'prototype']
                                  )
        // LenderMiniMap.prototype = MapSpy

        spyOn(Map, 'call').andCallThrough()
        spyOn(Map.prototype, 'setProjection').andCallFake(function(){});
        lender_mini_map = new LenderMiniMap({lender_id:1});
      });

      describe("build a map", function(){
        it("should have mini map attributes", function(){
          expect( lender_mini_map.scale ).toEqual(2000)
          expect( lender_mini_map.width ).toEqual(500)
          expect( lender_mini_map.height ).toEqual(800)

          expect( lender_mini_map.center ).toEqual([0, 55.4]);
          expect( lender_mini_map.rotate ).toEqual([4.4, 0]);
          expect( lender_mini_map.parallels ).toEqual([50, 60]);
        });
        it("should make async calls to the data source via d3", function () {
          expect(Map.call).toHaveBeenCalled();
          expect( Map.prototype.setProjection ).toHaveBeenCalled();
        });
      });

    });
