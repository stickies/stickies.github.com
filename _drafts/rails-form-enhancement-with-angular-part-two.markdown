---
layout: stickie
title: Rails form enhancement with AngularJS -- Part Two
meta: rails angular angularjs progressive-enhancement jquery javascript ruby nested_form
---
In my previous post we looked at using AngularJS to progressively enhance a Rails form.

In this article we will expand on that to include validating the form, handling state for the form and building nested form fields for has many models.

* We need to validate on the server and on the client so we will implement an active model based solution for the rails backend and an angular based solution for the front end.building a nested model for has many models.

### A nested model
First, lets create our nested model case:

{% highlight bash %}
rails g model article title:string description:string author:references
{% endhighlight %}

And add the has many reference and mass assignment to the author model in `app/models/author.rb`:

{% highlight ruby %}
class Author < ActiveRecord::Base
  has_many :articles # add this line
  accepts_nested_attributes_for :articles # and this line
end
{% endhighlight %}

Now run the migrations to update our schema.

{% highlight bash %}
rake db:migrate
{% endhighlight %}

The next thing to do is set up the controller and form for our new model. Add a partial at `app/views/authors/form/_articles.html.haml` with the fields:

{% highlight haml %}
#articles{'ng-controller' => 'ArticlesController'}
  = f.fields_for :articles do |a|
    .div{'ng-repeat' => 'article in articles'}
      .field
        = a.label :title
        = a.text_field :title, 'ng-model' => 'title'
      .field
        = a.label :description
        = a.text_area :description, 'ng-model' => 'description'
{% endhighlight %}

And then include this partial in the form view.

{% highlight haml %}
= render :partial => 'authors/form/articles', locals:{f:f}
{% endhighlight %}

_Now set up the respective controllers ... _

In `app/controllers/authors_controller.rb` build an instance of article in the new action:
{% highlight ruby %}
def new
  @author = Author.new
  @author.articles.build # add this line
end
{% endhighlight %}

And modify the angular code at `app/assets/javascripts/authors.js.coffee` to do the same.

{% highlight coffeescript %}
...

  class ArticlesController
    constructor: (@$scope)->
      $scope.articles = [{}]
      $scope.addArticle = ->
        $scope.articles.push( {} )
      $scope.removeArticle = (index)->
        $scope.articles.splice(index, 1)

...
    .controller( 'ArticlesController', ['$scope', ArticlesController] )

{% endhighlight %}
