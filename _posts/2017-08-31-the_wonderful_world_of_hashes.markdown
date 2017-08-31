---
layout: post
title:  "The Wonderful World of Hashes"
date:   2017-08-30 21:33:52 -0400
---


It's US Open time again.  For those of you who are not absorbed in the world of professional tennis, the US Open is the 4th and final Grand Slam of the tennis season.  So what has this event got to do with Hashes.

Tennis like all sports is immersed in data.  Professional tennis maintains a plethora of player statistics -- year turned pro, current ranking, matches won and lost for the season, career earnings, % of first service points won, aces, etc..  There are all sorts of statistics that can be gleaned from this vast array of data.  So, how could you easily retrieve data?  By putting all this data into a hash.

What follows is a very small sample of a Hash for pro_players: 

```
pro_players = { 
  roger_federer: { 
    current_ranking: 3, 
    career_matches_won: 1115, 
    grand_slames_won: 19 
    } 
    venus_williams: { 
      current_ranking: 9, 
      career_matches_won: 749, 
      grand_slams_won: 7 
    } 
  }
```


From this data structure, I can easily find Roger Federer's current ranking or Venus Williams' number of grand slams won.

```
pro_players[:roger_federer][:current_ranking]  

pro_players[:venus_williams][:grand_slams_won]
```

Looks like fun doesn't it!  If I want everything there is to know about Roger Federer, I would do the following:

```
pro_players[:roger_federer]
```
I could also add other players and their attributes to the pro-player Hash, iterate over it and find players with current_rankings in the top 10 or add the attribute of career_prize_money and find every player with career_winnings over $100 millin or anything else I want to have access to and use.  This is the power of the Hash and how we can take vast amounts of data manipulate that data and have some form of control over the complex world in which we live.

There is so much more to learn as I progress through the Full Stack Web Development Online Program,  but now that I've entered the world of Hashes--the future looks bright, but oh, so challenging!!


