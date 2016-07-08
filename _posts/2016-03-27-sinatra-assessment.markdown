---
layout: post
title:  "Sinatra Web App: Bhagavad-Gita As It Is Community Index"
date:   2016-03-27 18:20:20 -0500
categories: sinatra ruby app
---
![alt-text](https://farm2.staticflickr.com/1519/26007769742_bc1a5b7908.jpg)

This project was done as a final assessment in Learn.co's Fullstack Web Development course for the Sinatra section. We were tasked to create CRUD (Create, Read, Update, Delete) app of our choice. CRUD apps are sometimes referred to as Content Management Systems. I decided to make a community index for the world renouned book *"Bhagavad-Gita As It Is"*, by *His Divine Grace A.C. Bhaktivedanta Swami Prabhupada*. I was deeply transformed by the study of this book, and felt inspired to implement it into my study of programming. I've found that by combining the things you already enjoy with new things you are learning, it makes the process so much more engaging.

<!--more-->

## Planning:
Because this book requires a lifetime of study to actually absorb and implement it's teachings, it would be useful to create a sort of quick reference guide based on topics. It would also be nice if the information was accessible to everyone, even if you don't create a login. Here is my basic plan...

This program will allow Users to:

1. Login.
2. Create index's defined by a Topic name, which references a Verse's content.
3. View index's from all Users.
4. Edit any attribute of an index.
5. Be restricted from adding a Topic or Verse that was already made.
6. Logout.

Non-registered Users will be able to view all the data on the website, but will not be able to contribute in any way. (Even if they slap on '/new' or '/edit' to the base of the URL)

### Has Many to Has Many Relationships
The relationships between the classes will be as follows:

  - User has many Topics
  - Topic has many Users
  - Topic has many Verses
  - Verse has many Topics

  This will require two Join Tables:

  - UserTopics
  - TopicVerses

## It Isn't As Simple As It Sounds
Although the concept of this program is simple enough to understand, to get a computer to comprehend everything takes a load of work. My first hurdle came when I needed the User, Topic and Verse all to associate with each other upon creation. The next challenge was allowing every detail to be editted, and creating an edit form that shows you the current setup of your object. I learned a few tricks along the way. Here are a few:

### Disappearing Buttons
Want certain buttons to appear only when you want them to? Simply use a clever line of **erb** such as `<input type="<%= is_logged_in ? 'submit' : 'hidden' %>" value="Edit Topic">`
This makes your `type=` conditional based on the controller's helper method `#is_logged_in` which is simply:

     def is_logged_in
        !!session[:user_id]
     end

### Diplaying Error Messages
I used these two ways for displaying error messages:

**locals: method** - 
You can plug this in to your controller action.

    erb :"users/login", locals: {message: "Invalid username or password! Please try again."}

And this in your views.

    <% unless locals.empty? %>
      <%= message %>
    <% end %>

**redirect '/' method**
Plug this in to your controller action.

    message = "* You Must Be Logged In To Contribute*"
    redirect "/whatever_page?message=#{message}"

And this in your views.

    <%= @message %>

### Pop Out Window
This feature gives you a clickable link which reveals a window into another website. For example: look at the picture below. 

![alt text](https://farm2.staticflickr.com/1579/25494709784_be29dbe9e5.jpg)

**You see the link on the very bottom?  If you click that link, this opens in the same page:**

![alt text](https://farm2.staticflickr.com/1522/25826633940_3503ebf531.jpg)

I'll paste the code that I used for this feature below, and you can decode where to place your own information. It's all HTML, no CSS so it's a bit bulky, but it works!
    
    <a href="#" id="show_id" onclick="document.getElementById('spoiler_id').style.display=''; document.getElementById('show_id').style.display='none';" class="link">[Full View w/ Sanskrit, English Diacritics, Word-for-Word Translation & Purport]</a>
    <span id="spoiler_id" style="display: none"><a href="#" onclick="document.getElementById('spoiler_id').style.display='none'; document.getElementById('show_id').style.display='';" class="link">[Hide]</a><br>
    <div id="purport"> 
      <object type="text/html" data="http://www.vedabase.com/en/bg/<%=@verse.chapter%>/<%=@verse.verse%>" width="800px" height="800px" style="overflow:auto;border:5px ridge blue"></object>
    </div>
    </span>

### Requesting Authorization

I created a permissions model which would restrict users from editting information until they received permission and were added to a list of authorized users. The only unrealistic part about this model is that once you click the "Request Permission..." button, you are immediately granted access. In a real-world situation, I would implement a system that sends out an email to the Administrator, allowing him to either Accept or Deny the request. This opened another set of challenges, which was to create a system that checked the User's status in relation to many objects within the program.

## Conclusions

This project was very fun and challenging for me. Although I may have spent a little more time than I had wanted to, the experience of riding the waves over several days proved to me that I actually like this stuff. It also revealed to me why Web Developers have to work in teams. There is just too many things to think about to tackle it on your own. I also found that no matter how much pre-planning I tried to do, I always ended up tackling a new feature that I never planned to from the start. Regardless, the pre-planning probably saved me hours of time in the initial setup. Now that this is over, it's on to Ruby on Rails!!