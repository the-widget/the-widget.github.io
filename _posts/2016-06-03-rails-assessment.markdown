---
layout: post
title: "Rails Web App: Simple Sign Up"
date:   2016-06-03 09:25:20 -0500
categories: rails ruby
---
![ruby-on-rails](https://community-cdn-digitalocean-com.global.ssl.fastly.net/assets/tutorials/images/large/RubyonRails_twitter.png?1459466545)


#### Rails is awesome.

Rails is a popular MVC framework for programming in Ruby, and it's just plain great. I haven't even touched the tip of it's potential as of yet, but as I learn more about it, I can feel my excitement building. 

As part of the Learn.co curriculum, we are tasked to build a Content Management System, using "RESTful routes", complex forms and authorization (including login from an outside source ie. Facebook, Google, etc.

<!--more-->

This is my first self-created Rails app, and it's called Simple Sign Up. It's based off of the website SignUpGenious.com which provides a solution for organizational management. In it's current stage, it is not very useful, but nonetheless it has allowed me to understand the basic prinicples of Rails.

The idea is that it is used by an organization that plans various events, to recruit and organize workers or volunteers to fill necessary job positions. For example: Let's say you are organizing an 8 hour wedding expo. You may require people in charge of parking, serving food or beverages, hosts, and a clean up crew. This app allows an organizer to create tasks, and for workers/volunteers to sign up for various tasks.

My first task was to complete the models and the relationships that they have with one another. I built a Users, Events and Tasks model. The tricky part of this relationship is that there are two kinds of users, Organizers and Volunteers. A handy gem called 'devise' makes the job a little easier. Devise lets you create 'roles' which you can then use to control the flow of information presented to each user. For example, when an Organizer goes to the Tasks page, there will be a link to 'Create New Task'. Whereas, if a volunteer visits the same page, there will be a link to 'Sign Up For This Task'. 

I also wanted to implement a feature that allowed both Organizers and Volunteers to see how many positions have been filled for Tasks, and how many more positions needed to be filled for Events. This would give people a sense of where there is more 'need' for help. This was accomplished with a few simple instance methods (that calculated the amount of task positions vs. the amount of users who signed up for them), and a css selector which marked the text either green or red.

The most difficult part of this assignment was implementing Oauth for Facebook. I learned that a careful and thoughtful review of the documentation is essential for setting it up. So often we want a quick fix or setup, but that mentality ended up costing me so much time it's painful to think about.  Better to have a good understanding of the architecture of a gem before implementing it.

There were many more features I wanted to implement, but for the sake of my studying and urgent need to find a job, I decided to move forward in my Coding Journey.

