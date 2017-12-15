---
layout: post
title:      "Racquet-Finder: My CLI Data Gem Project"
date:       2017-12-11 12:30:42 -0500
permalink:  racquet-finder_my_cli_data_gem_project
---


Wow!  That's all that I can say after finally completing the coding of my CLI Data Gem Project.

 My project idea, being that I still play competitive tennis, is -- "select a tennis racquet from a website"  Sounds simple--right!  Well, sometimes it's simpler to say than actually implement. I decided that my domain model would be named racquet-finder.  As far as the process, I refactored more times than I could count and saved to GitHub more frequently than heart beats in a minute--as compared to having a heart attack if all the data was lost.  I aslo had to find a web scraping friendly website to have access to tennis racquet data--after the review of several, I found one that was compatible with my objectives, although not ideal.


I started the process after watching Avi's video on "The Daily Deal".  I wrote pseudo code for my CLI and started the scraping process with a Scraper Class.  My code was clunky and not very abstract with hard coding each and every webpage I could scrape data from.  I do believe my web scraping skills really improved with this exercise.  

However, while I had created a Scraper class and CLI class, I still needed to create my Ruby objects to make this CLI a Ruby object oriented program.  My grand plan was to start with selecting a Brand of Racquet, then a Model within a Brand and then a tennis racquet within that Model.  After a week or so of attempting to implement this and ending up with nested hashes in arrays and nested arrays in hashes- the light bulb went on and the plan was simplified:  just select a tennis racquet Brand and then a tennis Racquet within that Brand --no more middle ground--and then I was only dealing with arrays.  Since I could access individual racquets from the Brand webpage, even my web scraping was simplied.  This refinement of the scope of my project and assists on coding were directly the result of using Technical Coaches who expertly guided me as I hit road block after road block.

My project now consisted of a Scraper Class,  Brand Class, Racquet Class and a CLI Class.  Since I knew I could access individual racquets from the Brand webpage, my scrape_racquets method in my Scraper class would benefit by passing through the brand_url to access the individual racquet data--this was a move toward greater abstraction in my code that would also be reflected in my CLI--a user selects a Brand of tennis racquet and only that brand url would be scraped not all brand urls.  

Before I integrated my code by calling methods from different Classes within another Class (my collarborating objects), I used "pry" to test my code and ran my code within bin/console.  However, once my objects began collaborating I had to run my code from the terminal without pry (ruby bin/racquet-finder).  I ran into a permissions problems trying to execute the program.  With an assist from a technical coach, read/write permissions to my files were added.  However, while the permissions were changed, I ran into an error that I ultimately figured out as the "version" requirement.  My environment file (which was named racquet_finder.rb) orignally had "require "racquet_finder/version", but when I changed it to 'require_relative './racquet_finder/version' my program executed.  This type of roadblock also occurred when I added the Brand class and had my objects collaborate, but my program would not run.  I realized  I needed to require my Brand class.

For both the Brand class and Racquet classes, since the data provided on the website was large and clunky--not the best organized and not always containing the target data I needed, I added ranges to the Brand class variable and Racquet class variable so that a user would be presented with only the top 4 Brands of tennis racquets and top 6 of available tennis racquets by Brand.  


My CLI was refactored from the pseudo code I started with from the beginning of this process.  This was a fun experience as each error message or output provided valuable information in which I could learn.  I started with a basic, here is a list of brands, select a brand and here is a list of tennis racquets by brand.  I ended up with:  select a brand of tennis racquet, this is the brand you picked and here is a list of tennis racquets, now select one of the racquets in the list and display that selection, and do you want to select again?.  


Once everything worked, I did a happy dance.  I then realized I needed to write a blog (here it is), create a video for the user, and then upload all the urls to the Project page.  Finally, my project is complete.  
I now await my assessment for this project.














