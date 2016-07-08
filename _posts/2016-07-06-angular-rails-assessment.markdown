---
layout: post
title: "Angular - Rails Assessment Project"
date: 2016-07-06 13:05:00 -0500
categories: web development angular rails
published: true
---
![angular-js](https://upload.wikimedia.org/wikipedia/commons/thumb/c/ca/AngularJS_logo.svg/2000px-AngularJS_logo.svg.png
)

## Overview
This was by far the most challenging experience in programming so far. Learning about the different components that make up AngularJS was a fascinating process that made me very excited to use it to build a project, but when it came down to actually planning and building a project, I found myself a little spun. At this point in time the project is ready to be assessed because it fulfilled the minimum requirements, but is in no where near complete.

<!--more-->

## Angular and Rails
Getting Angular and Rails to play nicely together was very challenging at first. It took me a while to understand how to manage persisting information to the database while updating the view in real time. I got really stuck on the User Authentication error messages as well, I was using the Devise gem and trying to tie the error messages in with Angular's front end and it took me hours to understand what was going on. 

## Workflow
If there's one thing I want to work on more than anything, it's my workflow. I would love to have a walkthrough of the best practices of starting a project up to it's completion. I started by modeling everything out in Angular so that I could see what I was working with. After that I worked on tying that all in with Rails in order to persist the information to the database and retrieve it back to display in Angular. 

## Filters
I was able to implement multiple filters on the same list of posts. One filter would display all the posts that the current user has created, and another filter would sort the posts by title or author.

## Voting
The voting system is still not complete, but it does update the database in real time which is pretty cool. I still have to restrict the user to casting either one upvote or downvote. Currently the user is able to cast an unlimited amount of votes, which basically defeats the purpose of the voting system... Anyway I will get to work on it in the next phase of developing this project.

## Tags
As of now, tags can be displaying on the front end but there is no way to create them. I created some mock up tags on the back end and displayed them and see how it looked, but I have not yet implemented creating them. I used the angular-bootstrap-tags gem which has pretty good documentation but not enough examples to show how to really implement a good system for persisting and modifying tags.

## Conclusion
In conclusion I will say that I am both humbled and excited for the future. I'm humbled by the fact that this really is a complex world of programming and there is so much to learn, digest and apply. I'm excited for the future because I feel that with each day and each project I work on, I am becoming more and more proficient and intuitive as to how things work and how to fix things. It's amazing that even with so much stackoverflow and Google, there are still so many situation that are unique and really are a challenge to solve.
