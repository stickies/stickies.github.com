---
layout: stickie
title: Rails form enhancement with AngularJS -- Part Two
meta: rails angular angularjs progressive-enhancement jquery javascript ruby nested_form
---
In my previous post we looked at using AngularJS to progressively enhance a Rails form.

In this and the next few articles we will expand on that to include building nested form fields for has many models, validating the form and handling state for the form.

* We need to validate on the server and on the client so we will implement an active model based solution for the rails backend and an angular based solution for the front end.

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

### A model form service

Now that we have added a new set of nested fields with a nested key `author[article]`, the Angular code needs to be modified to handle the attribute keys in the same way that Rails would do. Otherwise we will end up with complex code or multiple end points doing virtually the same thing. To do this we wrap the form data model in a seperate service.

The service looks like this:

{% highlight coffeescript %}
  class FormData
    constructor: (data)->
      formData = data or {articles:[{}]}
      return formData

  angular.module('authorsApp.services', [])
    .factory('FormData', [FormData])

{% endhighlight %}

#### But for the sake of simplicity [look at the diff](https://github.com/stevemartin/rangular/commit/1cb48a3b24108ac6d8dbfee437601281919a350a) to get a better idea, and follow on from there.

### Adding and removing articles.

Now that we have a form service handling our form submission, we can start to think about adding and removing authors and how to implement these in JS and POP ( Plain Old Posts ).

We're going to start with the progressively enhanced version and the fallback to POP ( Plain Old Posts ) with the help of some directives. Directives are a feature of Angular that enable you to do stuff.

We open up our [authors partial](https://raw2.github.com/stevemartin/rangular/52da63e1b3332f1ceaf72122602b53b5593b763c/app/views/authors/form/_articles.html.haml) at `app/views/authors/form/_articles.html.haml`. And add the buttons that we are going to use, so that the template reads like this:

{% highlight haml %}
#articles{'ng-controller' => 'ArticlesController'}
  = f.fields_for :articles do |a|
    .div{'ng-repeat' => 'article in articles'}
      .field
        = a.label :title
        = a.text_field :title, 'ng-model' => 'article.title'
      .field
        = a.label :description
        = a.text_area :description, 'ng-model' => 'article.description'
      .actions
        = f.submit 'Remove Article', 'pe-remove-article' => true, 'ng-click' => 'removeArticle($index)'
  .actions
    = f.submit 'Add Article', 'pe-add-article' => true
{% endhighlight %}

Now we add the [angular directive code](https://github.com/stevemartin/rangular/commit/de883896784c130ab5ea8a9dbd44a17f013b1e19) to enable us to swap out the old submits that we added with the nice angular links.

{% highlight coffeescript %}
    .directive('peAddArticle', (@$compile)->
      return {
        link: (scope, element, attrs)->
          @html = '<a ng-click="addArticle()">Add Article</a>'
          @e = $compile(@html)(scope)
          element.replaceWith(@e)
      }
    ).directive('peRemoveArticle', (@$compile)->
      return {
        scope: {
          eventHandler: '&ngClick'
        },
        link: (scope, element, attrs)->
          @html = '<a ng-click="eventHandler()">Remove Article</a>'
          @e = $compile(@html)(scope)
          element.replaceWith(@e)
      }
    )
{% endhighlight %}

### Handling the form with Plain Old Posts.

This part pays homage to the concepts outlined in Hexagonal Rails.

[The new view](https://github.com/stevemartin/rangular/blob/c898f80dbf28973c52f9ca3690fa68fb3b13d9c5/app/views/authors/form/_articles.html.haml) after we have made some more modifications.

In order to add and remove articles without the use of angular or ajax, we will need to use some plain old posting with form state handling added in to give us an almost up to par user experience.

HTML forms only allow us to submit to one url ( without the use of javascript to modify the submission of course! ).

To notify the server that we are adding or removing articles we need to send back a parameter with the submission so that the server app can render the correct response. First, lets modify our enhanced submit buttons by adding an index and some name attributes that will get sent back to the server when the button is clicked.

{% highlight haml %}
#articles{'ng-controller' => 'ArticlesController'}
  - @index = 0
  = f.fields_for :articles do |a|
    .div{'ng-repeat' => 'article in articles'}
      .field
        = a.label :title
        = a.text_field :title, 'ng-model' => 'article.title'
      .field
        = a.label :description
        = a.text_area :description, 'ng-model' => 'article.description'
      .actions
        = f.submit 'Remove Article', 'pe-remove-article' => true, 'ng-click' => 'removeArticle($index)', name:"remove_article[#{@index}]"
        - @index += 1
  .actions
    = f.submit 'Add Article', 'pe-add-article' => true, name:'add_article'
{% endhighlight %}

### Form handler

Now that we are telling the server what we pressed when we submit the form, we can write a form handler to process the submission accordingly. Lets add some hexagonal magic to facilitate this.

First, create the form handler in `app/handlers/authors_handler.rb`, something like [this](https://github.com/stevemartin/rangular/blob/2bfd54c37159712415e4bc3f1883fc3295a8c8ef/app/handlers/authors_handler.rb) should suffice for now:

{% highlight ruby %}
class AuthorsHandler < Struct.new(:listener)
  attr_accessor :state_change, :params, :author
  def perform params, author
    @author = author
    @params = params
    set_request_type
    if state_change?
      manage_articles
      listener.recycle_form
    else
      if author.save
        listener.author_create_succeeded
      else
        listener.author_create_failed
      end
    end
  end

  def set_request_type
    @state_change = false
    @state_change = true if params['add_article'] || params['remove_article']
  end

  def state_change?
    state_change == true
  end

  def manage_articles
    if params['add_article']
      author.articles.build
    end

    if params['remove_article']
      articles = author.articles.to_a
      articles.delete_at(remove_article_id)
      author.articles = articles
    end
  end

  def remove_article_id
    params['remove_article'].keys.first.to_i
  end
end
{% endhighlight %}

Will will use this handler in the create action of our authors controller, which is the action that our form posts back to every time we press a button in the form. Based on the parameters that are sent back to the server, we can deduce whether or not the request is a form "state_change" or an actual attempt to save the data. First we refactor the create action so that it reads as follows:

{% highlight ruby %}
# POST /authors
# POST /authors.json
def create
  @author = Author.new(author_params)
  handler = AuthorsHandler.new(self) # pass the controller (self) in as the subscriber/listener.
  handler.perform(params, @author)
end
{% endhighlight %}

And now we add some callback methods to our controller subscriber clas, these will perform the correct response actions based on the outcome from the form handler.

{% highlight ruby %}
def recycle_form
   render :new
end

def author_create_succeeded
  respond_to do |format|
    format.html { redirect_to @author, notice: 'Author was successfully created.' }
    format.json { render action: 'show', status: :created, location: @author }
  end
end

def author_create_failed
  respond_to do |format|
    format.html { render action: 'new' }
    format.json { render json: @author.errors, status: :unprocessable_entity }
  end
end
{% endhighlight %}

Also, don't forget to add the articles attributes to the controller's `permit` attributes, you'll need to change the author_params method like so:

{% highlight ruby %}
def author_params
   params.require(:author).permit(:name, :email)
   params.require(:author).permit(:name, :email, articles_attributes:[:title, :description])
 end
{% endhighlight %}

Now we have a working *create* form that will submit new data to the server both asyncronously ( the progressive bit ) and traditionally the Plain Old Post bit.
