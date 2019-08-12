---
layout: post
title:      "Actyve - Rails Portfolio Project"
date:       2019-08-12 19:24:12 +0000
permalink:  actyve_-_rails_portfolio_project
---


Actyve is an activity tracking app where Users can post their workouts and get an overall summary of
their activities.

Not quite the comprehensive process to completing the app, but sort of the essentials I implemented when working through this project...

### Create the app
Create a new Rails application rails new actyve -T (-T flag to create without test framework)

### Use generators!
Rails generators save a lot of time when used correctly. Use the resource generator to build in the
code, minus the test framework again.

```
rails g resource *model name* *attribute name*:*data type* -T
```

The necessary files we will need from the generator is:
A migration file, which will create the database table
The model file which inherits from ActiveRecord::Base
A controller file that inherits from ApplicationController
An empty view directory where we can build our model views
A call to all resources in the routes.rb file

### Migrate the data
If rails recognises your resource generator command, then the migration file should already be set up,
but just double check all the data is correct and then:

`rails db:migrate`

This will migrate our tables into our SQLite database.

### Define your relationships
In the model file, define the relationships between each model.

app/model/user.rb
```
has_many :activities
has_many :exercises, through: :activities
```

We can place our model validations here too:
```

validates :username, :email, presence: true
validates :username, :email, uniqueness: true
```

### Configure Routes

Although the resource generator will give us a full resources call for all models, we do not necessarily
need them all, and we can configure them to only generate the routes we use. For example, the Exercise
model will have 4 hard coded types, and therefore will not need a create, update or delete route, and we
can code this like so:

config/routes
`resources :exercises, only: [:index, :show]`

Another requirement for the project is to have a resource that is nested under a parent. The Activity
model will require an exercise_id as its foreign key, so naturally we can nest it under the Exercise
resource:

```
resources :exercises, only: [:index, :show] do
    resources :activities
  end
```

We can also go ahead and explicitly define our routes for use in our controllers:

config/routes.rb
```
root 'sessions#home' --> This will be our default view for localhost:3000
get '/signup' => 'users#new'
get '/login' => 'sessions#new'
post '/login' => 'sessions#create'
```

### Omniauth

Aside from the standard login, I implemented login through github via the omniauth-github gem. In the
gemfile I added:

```
gem 'omniauth'
gem 'omniauth-github'
```

I ran a bundle install and then created a omniauth.rb, to place my OAuth credentials:

config/initializers/omniauth.rb
```
Rails.application.config.middleware.use OmniAuth::Builder do
  provider :github, '63523f71f32d7b36292b', 'a39a292dce5e7900101706147fde286945cf0d18'
end
```

add the following to the routes.rb:

`get '/auth/github/callback' => 'sessions#create'`

and then the following will be coded into the sessions controller:

```
def create
  if auth_hash = request.env["omniauth.auth"]
    @user = User.find_or_create_by_omniauth(auth_hash)
    session[:user_id] = @user.id

    redirect_to activities_path
  else

  standard login...
end
```

### Code the views

For users to be able to start adding their activities, we need to add forms in our views

```
<%= form_for [@exercise, @activity] do |f| %>

  <%= f.hidden_field :exercise_id, :value => @exercise.id %>
  <%= f.hidden_field :user_id, :value => current_user.id %>

...
```

it is a nested resource, so we add the parent model (@exercise) followed by the child (@activity). We
also add the exercise_id and user_id foreign keys as hidden fields, followed by the user submittable
attributes:

```
...

<%= f.label :title, "Title your activity" %>
  <%= f.text_field :title %>

  <br><br>

  <%= f.label :location %>
  <%= f.text_field :location %>

  <br><br>

  <%= f.label :distance, "Distance (km)" %>
  <%= f.number_field :distance, :step => 0.01 %>

  <br><br>

  <%= f.label :calories %>
  <%= f.text_field :calories %>

  <br><br>

  <%= f.label :hour, "Hour(s)" %>
  <%= f.number_field :hour %>
  <%= f.label :minute, "Minute(s)" %>
  <%= f.number_field :minute %>

  <br><br>

  <%= f.label :comment %>
  <%= f.text_area :comment, rows: 2 %>

  <br><br>

  <%= f.submit %>

  <% end %>
```

### CRUD in the controller

Add in the logic so that the controller can translate the data passed from the views and persist it
to the database, providing it passes all the validations:

```
def create
  @activity = current_user.activities.new(activity_params)
  if @activity.save
    redirect_to exercise_activity_path(@exercise, @activity)
  else
    render :new
  end
end
```

### Helpers

Notice in the create method we have current_user. In the application_controller.rb we can define a
private method:

```
private

def current_user
  @current_user ||= User.find_by(id: session[:user_id])
end
```

This is logic we can use throughout the application, and calling it will always return the user that is
currently logged in.

We also have a is_logged_in? and authenticate_user! method that will check to see if a user is logged
in, and redirect a user to the login view if they are not.

### Partials

We use partials to DRY up our code. If we find we are repeating code constantly, we can throw them into
a partials form and call them in our views.

Rather than code in error logic for every form view, we can place the code in an errors.html.erb:

```
<% if object.errors.any? %>

  <h3>
    <%= pluralize(object.errors.count, "error") %>
    prohibited this from being saved:
  </h3>

  <ul>
  <% object.errors.full_messages.each do |msg| %>
    <li><%= msg %></li>
  <% end %>
  </ul>
<% end %>
```

We can then call this at the top of each view:

`<%= render partial: 'layouts/errors', locals: {object: @activity} %>`

we define the "locals" to be the model we are referencing.

### Scope Methods

I wanted to be able to query user data and display a total summary of all users activities and a summary
of a users activities within the current month. In activity.rb:

```
scope :user_activities, -> (user) {where(user_id: user)}

scope :this_month, -> { where(created_at: Time.now.beginning_of_month..Time.now.end_of_month) }
```

and then to implement this in the user controller:

```
def show
  @user = User.find_by(id: params[:id])
  @activities = Activity.user_activities(@user)
end
```

This was my second attempt at coding my project. Like others, I fell into the trap of being too ambitious/overcomplicating things, even though I thought I never would. I guess it is easy to think of new ideas and believe there are easy ways to do it, only to realise the more code you add, there more it can have a butterfly affect on other methods. Keep it as simple as possible to meet the requirements, and then upgrade as you wish.
