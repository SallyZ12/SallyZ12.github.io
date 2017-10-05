---
layout: post
title:      "Are Objects Really Metaphors for Life"
date:       2017-10-05 22:43:51 +0000
permalink:  are_objects_really_metaphors_for_life
---


As I progress through the curriculum, there are certain concepts that are emphasized over and over again.  I guess, when you begin to internalize these concepts, becoming a programmer seems to be closer to reality.  Just like Star Trek there is a primary directive in Ruby programming -- Objects are metaphors for life!

What does this actually mean?  You can create a type of something and many of those somethings and define the look and feel and finally the behaviour of those somethings.  In Ruby, we speak of Classes and Instances as the type of something and the many of those types, respectively.  For each of those Instances we can define characteristis or attributes and behaviors or methods to bring this instance into the world.  But, we can't forget that the Class or Type inherently has its own set of characteristics or behaviors--such as a Dog or Car is unique to each other by virtue of being a Dog or a Car.

Since we are creating something new that is brought into this world, we need to take special care in this process and define those characteristics and behaviors that can survive and succeed in this modern world.

Hurricanes have been very topical lately.

I put together some code that gives birth to named Hurricanes and then by using the method category_wind_speed you can see the range of wind associated with that hurricane category.  Since storms can change categories as they move over warmed oceans and increase in intensity or decrease in intensity as they begin to move over dry land --you want to be able to enter the category for that specific storm and see the range of winds associated with that category.  In addition, you want to see all of the Hurricanes that were born-- so the class method self.all was defined as well.

```
class Hurricane
  @@all = []
  attr_accessor :name, :category

  def initialize(name)
    @name = name
    @@all << self
  end

  def self.all
    @@all
  end

  def category_wind_speed(category)
    if category == 1
      "Wind Speed between 74 mph and 95 mph"
    elsif category == 2
      "Wind Speed between 96 mph and 110 mph"
    elsif category == 3
      "Wind Speed between 111 mph and 129 mph"
    elsif category == 4
      "Wind Speed between 130 mph and 156 mph"
    elsif category == 5
      "Wind Speed >= 157 mph"
    else  
      “The Apocalypse"
    end
  end
end

```

Instances of Hurricanes can be created by:
sandy= Hurricane.new("Sandy")
irma = Hurricane.new("Irma")
harvey = Hurricane.new("Harvey")

To see the collection of Hurricanes - enter Hurricane.all
an instance of an object is delivered along with their name since the name was instantiated at their birth.

To see the wind speed for a particular Hurricane category (in this example Hurricane 4), then enter
sandy.category_wind_speed(4) and the output is

 "Wind Speed between 130 mph and 156 mph".
 
 So indeed, Objects are metaphors for life and while this was a simple example you can extrapolate and complicate many real world examples -- which I hope to do as my programming experience continues through The Flatiron School.

.
