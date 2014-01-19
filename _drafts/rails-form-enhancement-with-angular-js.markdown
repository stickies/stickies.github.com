---
layout: stickie
title: Rails form enhancement with Angularjs
meta: rails angular angularjs progressive-enhancement jquery javascript ruby
---
### Preface
These days single page apps are all the rage. There are many technologies emerging that enable slick UI without having to do much on the server.

Historically, Rails used the prototype and jquery libraries to enable form enhancement.

Angularjs is a frontend javascript framework developed by Google, it acts like an MVC framework but at a different layer of abstraction. Angular is a declarative tool.

__A few words on Angularjs ...__
> *AngularJS is a toolset for building the framework most suited to your application development. It is fully extensible and works well with other libraries. Every feature can be modified or replaced to suit your unique development workflow and feature needs. Read on to find out how.* - __angularjs.org__
__A few words on progressive enhancement ...__
> *Progressive enhancement is a strategy for web design that emphasizes accessibility, semantic HTML markup, and external stylesheet and scripting technologies. Progressive enhancement uses web technologies in a layered fashion that allows everyone to access the basic content and functionality of a web page, using any browser or Internet connection, while also providing an enhanced version of the page to those with more advanced browser software or greater bandwidth.* - __Wikipedia__

#### First, implement a basic rails scaffold.
For the purposes of this tutorial we will be using Rails 4.0.1 and Angular version 1.2.9.

{% highlight bash %}
rails new rangular
cd rangular
echo "gem 'haml-rails'" >> Gemfile
bundle
rails g scaffold author name:string email:string
rake db:migrate
rails s
{% endhighlight %}

This will generate a basic rails app ( with author model, database and form views ) and boot the basic rails server.

Now, navigate to [the new author form](http://localhost:3000/authors/new){:rel='nofollow'} in your browser. You will see a basic non progressive-enhancement form that you can submit to.

#### Next, *angularize* the app.
1) Open the file: `app/assets/javascripts/author.js.coffee` and add the following code:

{% highlight coffeescript %}
app = angular.module 'authorsApp', []
{% endhighlight %}

2) Now, create a new layout template for the authors resource by copying the existing one...

{% highlight bash %}
cp app/views/layouts/application.html.erb app/views/layouts/authors.html.erb
{% endhighlight %}
*By default rails will look for a layout with the same name as the controller before it falls back to application layout.*

3) Open the file: `app/views/layouts/authors.html.erb` and add the angular cdn to the header:

{% highlight html %}
<head>
...
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.9/angular.min.js"></script>
  <script src="https://ajax.googleapis.com/ajax/libs/angularjs/1.2.9/angular-route.js"></script>
...
{% endhighlight %}
4) then rewrite the `<body>` tag so it reads:

{% highlight html %}
<body ng-app='authorsApp'>
{% endhighlight %}

You can see the file [in that state](https://github.com/stevemartin/rangular/blob/e32b29a3073a2c8f50f48a60bdcceaacd7a49cd4/app/views/layouts/authors.html.erb).


### Enhance!
Now we can begin to enhance the form.

1) Open the file: `app/assets/javascripts/author.js.coffee` and replace the contents with the following code:

{% highlight coffeescript %}
do (angular)->
  angular.module 'authorsApp', [
    'authorsApp.controllers'
  ]

  class FormController
    constructor: (@$scope, @$http)->
      $scope.submitForm = ->
        $http.post('/authors')

  angular.module('authorsApp.controllers',[])
    .controller 'FormController',['$scope','$http', FormController
    ]
{% endhighlight %}

Now edit the cooresponding form template `app/views/authors/_form.html.haml` like so, you can [see the diff here](https://github.com/stevemartin/rangular/commit/a5986687b1fe3c3d4ab01bcb28e440f4bc1db59a#diff-8fd183f381211860fbd88f5416b717ecL1):

{% highlight haml %}
= form_for @author, html:{'ng-controller' => 'FormController', 'ng-submit' => 'submitForm()' } { |f|
  - if @author.errors.any?
    #error_explanation
      %h2= "#{pluralize(@author.errors.count, "error")} prohibited this author from being saved:"
      %ul
        - @author.errors.full_messages.each do |msg|
          %li= msg
  .field
    = f.label :name
    = f.text_field :name, 'ng-model' => 'name'
    %span{'ng-bind' => 'name'}
  .field
    = f.label :email
    = f.text_field :email, 'ng-model' => 'email'
    %span{'ng-bind' => 'email'}
  .actions
    = f.submit 'Save'

{% endhighlight %}
* implement form service.
* implement nested model and its relation to ( ng-repeat ).

#### Citing:
* [angular-coffee](http://alxhill.com/blog/articles/angular-coffeescript/)
