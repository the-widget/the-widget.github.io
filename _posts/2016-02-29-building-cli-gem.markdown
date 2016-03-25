---
layout: post
title:  "Building A CLI Gem: Making of RunSwimHike NYC"
date:   2016-02-29 15:18:20 -0500
categories: ruby gem cli
---
![alt text](https://farm2.staticflickr.com/1590/25234838110_ef39acc1fb.jpg)


One of the Final Projects in Learn.co's Object Orientation Module is to build your own CLI Gem. We are required to publish our Gem on RubyGems and make it available for anyone to try. Feel free to check out the source code here: [RunSwimHike NYC](https://github.com/the-widget/run-swim-hike-nyc)

## **Research Phase:**

Previous to this project we studied Object Oriented Programming and site scraping using Nokogiri. I heard fellow students complaining about their gems quickly 'breaking' due to changes in the website, so I wanted to find a relatively stable source. I remember hearing about NYC Open Data which provides over 1300 data sets on everything from public toilets to real-time traffic speed detectors. I went through several dozens of data sets until I decided I would settle on places to get some exercise. Soon after, I decided on the name and subject for my CLI gem. "RunSwimHike NYC". NYC Open Data provided these data sets in 'xml' format, which I had no previous experience of scraping, but it seemed easy enough.

## **Planning Phase:**

I wanted this gem to be flexible, organized and useful. Since there was a lot of information to deal with, I wanted to divide the menus up into three main categories: naturally... Run , Swim and Hike. After that was established, the user should pick one of five boroughs. After viewing a numbered list of names and locations, the user should be able to get more information about the facility and have direct access to a Google Maps listing. Here's a basic overview of the workflow:

- Welcome!
  - Run
  - Swim (should offer Indoor/Outdoor options)
  - Hike
    - Bronx
    - Brooklyn
    - Manhattan
    - Queens
    - Staten Island
        - :facility list:
        - :individual facility info:
        - 'open a Google Map listing'
        - 'go back to facility list'
        - 'reset program'
- 'option to quit/reset at anytime'

I decided to create a main CLI class, and a seperate class for each activity along with it's own scraper class.

- CLI
  - Run Scraper
  - Run Class

  - Swim Scraper
  - Swim Class

  - Hike Scraper
  - Hike Class

## **Strategy:**

1. Get **everything** working with the Run/Run Scraper class including menus and all features. Then, copy and apply to the next two subjects. This strategy really paid off in the end, and completing the last two sections was like a breeze.

2. Scrape, collect, and create instances of all objects upon the user's choice of Run, Swim or Hike. Iterate through an array of instances to display information.

3. Use class methods to allow all instances to be iterated over easily.


## **Lessons Learned:**

After figuring out how to parse xml files, my biggest challenge was designing a system that allowed the user to return to a previous window. This was especially challenging for the Swim class, because I wanted to offer the option for Indoor/Outdoor pools, which meant I needed to interate through all the instances of Swim again. Here were some takeaways:

1. Pre-planning goes a **long** way.
  * Put yourself in the shoes of the user. Become the user and see what function you would like to see. Try to predict what would annoy/please you.

2. Commit more frequently.
  * I found myself getting so caught up in progamming that I wasn't committing until a huge chunk of the program was finished. At one point, my MacBook Air froze up and I did a hard reset. Luckily, none of my progress was lost, but luck doesn't always take your side.
3. Take more notes along the way.
  * There's a huge amount of learning that happens during the programming process. If these valuable lessons can documented, you can further solidify your own understanding and share it with others.

## **Little Extras:**

Adding little personal touches to a CLI program can make it feel more human. Here were a few extra features that I incorporated into this program:


### Spinny
This is a spinning icon that runs in your cursor space while you wait for information to load. Here is the source code for that:

    def self.spinny  
    n=0
    a=["-","\\","|","/"].cycle do |a|
      print a
      print "\b"
      n+=1
      sleep 0.1
      break if (n % 6).zero?
    end


### Sleep
I used the `sleep` keyword to mimick loading time, but I used it so allow the user to see the spinny, as well as to animate a list of parks being printed. Instead of it just appearing suddenly, it puts 4/100ths of a second between each `puts`. Here is an example from my Run class:

    def self.display_borough(borough)
    spinny
    index = 0
    puts " \nHere are the Running Tracks in #{borough}: \n "
    spinny
    parks.each do |track|
      if track.borough == borough
        parks << track unless parks.include?(track)
        puts "#{index+1}. #{track.name} - #{track.location}"
        index += 1
        sleep(0.04)
      end
    end


### Farewell
This bids the user farewell with a unique message before quitting the program. I set a class variable equal to an array of farewell messages, and used the `.sample` method to choose one at random. Here is the code for my `quit` method:

      @@farewell = ["Have a nice day!", "Take care of yourself!", "You'll never regret some good excercise!", "Have fun!", "Thanks for checking us out!", "Keep running, swimming and hiking!", ":)"]

      def self.quit
        puts " \n"
        abort("#{@@farewell.sample} \n ")
      end
