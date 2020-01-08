---
layout: post
title:      "Gremlins or Fetch - Which Alien Is It?"
date:       2020-01-08 15:44:56 +0000
permalink:  gremlins_or_fetch_-_which_alien_is_it
---


As a recent graduate (2019) of the Flatiron School's Online Software Engineering Bootcamp, I was determined to continue to code so I wouldn't forget all the fabulous skills acquired during the program and because I had unfinished business with my Final Project.  While I had a successful assessment, there were a number of things I wanted to refine or fix that were just a constant annoyance in my world of perfection.  As you know, no App is ever finished.  There are always things that can look better, increase functionality, and enhance the user experience.

My Final Project was a Tennis Court Reservation App (“Court Time”) where one or more Tennis Clubs can be accessed via a link and then a court selected to book playing time based on date and time.  While initially I had a bunch of components in App.js, I streamlined my code and efficiently placed various components in their own Containers and then passed through those Containers in App.  What I didn’t realize is I left a little gremlin in the process. 

When I first initiate the App (from scratch—start over in the terminal, find the correct file paths, start the servers) I would get an error message of “TypeError” in the Club component.  However, when I did a browser refresh the error resolved and my home screen displayed with no complications.  Since the error only occurred when starting fresh, I really didn’t give it a lot of thought – everything else worked smoothly.  

Then came that nagging perfectionism in me --- there must be a reason for the error.  Calling on my debugging skills I looked at the backend server information.  Sure enough, there was an error occurring during the fetch processes.  I checked my code and realized that when I moved the Club Component to the Club Container from App, I left in the ComponentDidMount lifecycle to the fetchClubs action creator.  Therefore, when starting from scratch a double fetch to the fetchClubs action creator occurred and my App was very confused. To solve this issue, I removed the fetchClubs action creator from the Club Component so it only got called in App.

Conclusion is that there were no Gremlins, only a double fetch!!  The moral of the story is fetch once and not twice, it will save you from hunting down Gremlins.

