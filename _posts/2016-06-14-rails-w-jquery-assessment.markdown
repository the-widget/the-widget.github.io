---
layout: post
title: "Rails App w/ jQuery Front-End"
date: 2016-06-14 09:15:20 -0500
categories: ruby rails javascript jquery
published: true
---

![rails-jquery](http://blog.justprofessionals.com/wp-content/uploads/2010/02/ruby-rails-jquery-release.jpg)

## Simple Sign Up (part 2)
In a previous blog post I covered my first Rails App called Simple Sign Up, which was the Rails Assessment project for [Learn.co](https://learn.co/). In the next section, Rails & Javascript, our assessment project was to take the same Rails App and modify it by implementing jQuery, for the front-end. We were also tasked to create an internal API in which all the jQuery calls would be made to. Sound a little confusing? Let's break it down in smaller bites.

<!--more-->

# What is an API?
In brief, an API (Application Programming Interface) is a resource that allows one system to access the information on another system using standardized protocols. I sometimes visualize an API as a sort of index that you might find at the back of an encyclopedia, which has a wealth of text and images. If you are looking for a specific topic or resource, the index organizes everything, allowing you to find the corresponding pages within seconds. Imagine that someone is trying to create an iPhone app that helps you locate the nearest pizza place. Instead of finding and documenting every pizza place, the easier route would be to use those resources that already exist, such as Foursquare or Yelp. By accessing Yelp's public API, that wealth of information can be easily accessed and consumed at your disposal.

# About jQuery
jQuery is a Javascript library (of prewritten functions) which allows programmers to easily manipulate data in fascinating ways. Take Facebook for example: When you create a Post, the page never reloads, but rather seamlessly disappears and reappears on your wall. Or, when you hover the cursor over someone's name, a small box appears revealing details about this person. This is the magic of Javascript. In this project, the main use of jQuery is in its ability to make AJAX calls to the internal API, select HTML components to manipulate, and display that information on the front-end. In short, this allows the application to access, manipulate and display information without any page redirect.

# The Process

#### Creating an Internal API
Thanks to the Rails community, we have an awesome Ruby gem called `active-model-serializers` which helps us generate JSON from our object models. Here's what my Event Serializer looks like: 
*`app/serializers/event_serializer.rb`*

```
class EventSerializer < ActiveModel::Serializer
  attributes :id, :title, :description, :date, :start_time, :end_time
  belongs_to :user
  has_many :tasks
end
```
As you can see, the `attributes` keyword allows me to control what data I want serialized into JSON. This simplifies our models which may contain more information than we need. The Event Serializer also accept the keywordsm `belongs_to` and `has_many`. This will take the serialized data of the related models and nest them in an array. In the screenshot below you will see an array of Tasks, which contain an array of Users. 

Here is the corresponding `show` action of the Event Controller
*`app/controllers/events_controller.rb`*

```
def show
  @event = Event.find(params[:id])
  respond_to do |format|
    format.html { render :show }
    format.json { render json: @event}
  end
end
```

This choice of `|format|` allows me the flexibility to access my standard `show` template, as well as my new JSON resource.
Now, when I navigate to `events/:id.json` I will see something like this:

![JSON formatted view](https://c8.staticflickr.com/8/7480/27596849751_c6197b3449_z.jpg)

Here is an organized and simple resource for everything I need to access about my Event. In this JSON format, it is very easy to parse the data and move that information into my views via jQuery

#### Javascript Model Objects

Now that the API is created, I can make an AJAX call to my API and create Javascript Models in order to display the data on the front-end without redirecting the page. Here is a shortened example of what that might look like: [Actual Source Code on GitHub](https://github.com/the-widget/simple-sign-up/blob/master/app/assets/javascripts/events.js)
*`app/assets/javascripts/event.js`*

```
function Event(id, title, date, description, start_time, end_time, tasks, user){
  this.id = id
  this.title = title
  this.date = date
  this.description = description
  this.start_time = start_time
  this.end_time = end_time
  this.tasks = tasks
  this.user = user //Organizer

  this.display_each_task = function(){
    $.each(this.tasks, function(i, task){
    tasks_html = tasks_html.concat("<a class='js-showTask' href='#'" + "data-id=" + task.id + " data-eventTitle=" + "'eventTitle + "'" + " data-eventId=" + task.event_id + ">" + task.title + "</a><br>");
    });
  };
};

$.get("/events/" + eventId + ".json", function(data) {
  var event = new Event(data['id'], data['title'], data['date'], data['description'], data['start_time'], data['end_time'], data['tasks'], data['user']);
  event.display_each_task();

  $('.event-tasks').html(tasks_html);
};
```
The first block of code assigns the models attributes to the Event object. Simple enough right?

The next block of code is the method `display_each_task`, which iterates though the array of `Event.tasks`, and concatenates the information into a HTML string called `tasks_html`. This HTML string can now be applied to the current view through a [jQuery selector](https://api.jquery.com/category/selectors/).

The final block of code in this example makes a `GET` request to the JSON resource (in the above screenshot), creates a new instance of Event, and calls the method `display_each_task` to concat that HTML string for this Event. Then the jQuery selector `$('.event-tasks')` replaces the HTML in the selected `<div>` with `tasks_html`, displaying it instantly on the page.
The actual source code for the app is a little more complicated, but follows this basic flow.


# Challenges

#### Permissions

One of the challenges I faced was controlling permissions. Using the gem `devise` I set up two main User roles, organizers and members. The admin role has complete control over the entire app. Organizers can create, edit and delete Events and Tasks, and Members can sign up for Tasks.

*`app/models/user.rb`*

```
class User < ActiveRecord::Base
  devise :database_authenticatable, :registerable,
         :recoverable, :rememberable, :trackable, :validatable,
         :omniauthable, :omniauth_providers => [:facebook]

  enum role: [:admin, :organizer, :member]

```
With `html.erb` files, it is very easy to apply permissions to your views using helper methods.

```
<% if organizer_permissions? %>
  <%= link_to("Create New Event", new_event_path, :id => 'new-event')%>
<% end %>
```
I needed to get more creative to implement this same concept in Javascript. I'm sure there are better solutions to achieving this goal, but here is what I decided to do:

- Since the Event `show` page is rendered using an `html.erb` file, only an Organizer will have HTML code in the `.page-nav` `<div>`
- Using this boolean function (`function isEmpty( el ){return !$.trim(el.html());};`) allowed me to test whether or not the `<div>` was empty, and would indicate whether the user was an Organizer or a Member. 

```
if (!isEmpty($('.page-nav'))) {
  $('.page-nav').html(eventNav);
;
```
I used the above function to display the `eventNav` only to Organizers, allowing them to edit and delete Events.

#### Creating An Array of Unique Objects in Javascript

In the Event `show` page, I list all the participants(users) for the Event. These participants are recognized by their association with a Task that `belongs_to` that Event. Since some Users are signed up for multiple Tasks within the same Event, in order to collect a list of unique Users, there is some logic involved. While Ruby's `.uniq` method would work perfectly in this situation, we are dealing with Javascript. Here is how I implemented this logic.

```
var users = {} //Workers
this.display_each_user = function() {
  $.each(this.tasks, function(i, task){

  // assigning object keys (instead of Array.push) causes an overwrite, making it unique.

    $.each(task.users, function(i, user){ 
      users[user.id] = user.name
    });
  });
  for(var i in users){ // Loops through the keys in 'users' to concat
    users_html = users_html.concat("<li class='users'" + "data-id=" + i + ">" + users[i])
  };
};
``` 
The comments made in the above code describe what's going on, but to restate the logic I will explain it in this way. By iterating two levels deep into the Users of each Task, I assigned the `user.id` as the key, and the `user.name` as the value `{:user.id => user.name}`. As it iterates through the Users, if the same User appears it will recognize the existing key and update the value rather than creating a new object. Then the last two lines of code iterates through each key in the `users` hash and concats an HTML string which lists the names.

# Conclusion

After completing this project, I feel a sense of satisfaction to see a working result of all the effort I put in. Javascript animations are so nice to see, and they create such a fluid feeling to the web app, an effortless page load. At the same time, I understand that this was just a tiny piece of the pie. My other links do not load via jQuery and break the smooth flow that exists in the `index` and `show` pages. For example, if you updated an Event you would not be redirected to the Event via jQuery, but would be redirected to the standard `.html` format in the controller. I also feel a need to understand Javascript and Ruby in a deeper way. When I look at the source code, it looks quite crowded and needs to be cleaned up. Overall I had so much fun with this project, and am looking forward to learning Angular JS in the upcoming module.















