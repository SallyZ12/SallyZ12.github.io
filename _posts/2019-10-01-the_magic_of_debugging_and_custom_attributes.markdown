---
layout: post
title:      "The Magic of Debugging and Custom Attributes!"
date:       2019-10-01 15:34:08 +0000
permalink:  the_magic_of_debugging_and_custom_attributes
---


At the beginning, the idea of building from start to finish a Rails backend with a React/Redux frontend seemed insurmountable---so many pieces of the puzzle, so many unknowns. However, when you approach this Project methodically with a clear plan and an understanding your data,  the building blocks allow for success.

How many times have you drawn a picture of what you want your App pages to look like, with the flow from one screen to the next, and with the expection of "I'm inputting or grabbing data from "x "and I want it to look like "y" and end up on page "z" "?  If you know what your data is expexcted to be from the beginning of the pipeline to the end, it is the foundation for successful coding.  How can you event test that your data remotely reflects what you input and then output when you don't have workable code to see in your browser?  The answer:  "Debugging"



 **BACKEND**
 
 **Pry**


Pry is a fabulous tool to see what data is coming into your database and to see if your instance variables or methods are woking properly.

To check and see if your data inputs are available to be persisted into your database, stick a binding.pry in your create action of the relevant controller--in my case, I'm working on reserving a tennis court at a specific tennis club. Check to see "params": 

``pry(#<Api::V1::ReservationsController>)> params
=> <ActionController::Parameters {"court_id"=>5, "day"=>"Oct 03 2019", "hour"=>"9:00 am", "rate"=>"", "rate_type"=>"", "confirmId"=>"", "user_id"=>"1", "confirmID"=>768036, "controller"=>"api/v1/reservations", "action"=>"create", "reservation"=>{"user_id"=>1, "court_id"=>5, "rate_type"=>"", "hour"=>"9:00 am", "day"=>"Oct 03 2019", "confirmID"=>768036, "rate"=>""}} permitted: false>``
``



You can see that my data input from my front end is making it to my create action.  
Also, I can test to see if I'm correctly reflected as logged_in.

```

pry(#<Api::V1::ReservationsController>)> logged_in?
  CACHE User Load (0.0ms)  SELECT  "users".* FROM "users" WHERE "users"."id" = ? LIMIT ?  [["id", 1], ["LIMIT", 1]]
  ↳ app/controllers/application_controller.rb:10
=> true

```

If I want to see if my instance variable contains the correct attributes, I can type in @reservation 


```

 pry(#<Api::V1::ReservationsController>)> @reservation
=> #<Reservation:0x007fb324500828
 id: nil,
 user_id: 1,
 court_id: 1,
 rate_type: "",
 hour: "9:00 am",
 day: Tue, 08 Oct 2019,
 created_at: nil,
 updated_at: nil,
 confirmID: "681174",
 rate: nil>

```

This information tells me I've got the correct data populating my instance variable and I'm on the correct track.


**Rails Console**

If you have many different associations, with several has_many and has_many, through relationships, your best friend is the rails console.  This is where you can test those associations and lock down your models.  Since my app is a tennis court reservation system, focus of the app is a reservation. 

The basic association is a reservation belongs to a court and belongs to a user.  But we know that a court belongs to a club, and a club has_many reservations, through a court. Sounds confusing, but with the magic of the rails console, we can test those associations.

While my reservation does not itself include a club, we can find the club and the club's name with the use of associations.

```

 Reservation.first.club.club_name
  Reservation Load (0.2ms)  SELECT  "reservations".* FROM "reservations" ORDER BY "reservations"."id" ASC LIMIT ?  [["LIMIT", 1]]
  Club Load (0.2ms)  SELECT  "clubs".* FROM "clubs" INNER JOIN "courts" ON "clubs"."id" = "courts"."club_id" WHERE "courts"."id" = ? LIMIT ?  [["id", 7], ["LIMIT", 1]]
 => "Delray Beach Tennis Center" 

```


This result is of critical importance because it now validates my associations amongst models.   This methodology also is meaningful because we can leverage off of this relationship and create a custome attribute in our serializer which brings the power of these associations to the front end.  To try and render a reservations's club name in the front end is complex and requires multiple lines of code.  However, I can create a custom attribute that I can use  in the frontend similar to any other attribute I define as accessible to the front end.

**Custom Attributes**

In my ReservationSerializer, in addition to the standard attributes defined in my schema, I created a custom attribute that allows access to a reservation's club name:

```
def reservation_club
      self.object.club.club_name
  end


```

I then add this custom attribute  reservation_club to the attribute list .  That's it, it's as simple as that.  I  can now access in the frontend a reservations' club name as easily as I can access a reservation's day and hour  and call {reservation.reservation_club} in my return statement.





**FRONTEND**


 **Console.log**

When you turn your attention to the frontend, console,log can be your best friend.   You can place a console,log just about any place.  I often placed a console.log in a functional component to see if  I was correctly passing "props" from the parent component.  You can also look at your React Dev tools to see this result as well. Console.log provides information on the specific variable  you specify.  You can drill down on an object and see the data passed from a parent component.  However, to gain more traction in a dynamic way use "debugger", which is a  complementary tool to console.log.



**Debugger**

Using debugger in conjunction with the console is a powerful tool. Add a debugger to your code and run your code and it will stop at that debugger point  and you will be able to hover over a variable in the source code in the development tools or go to the console and type in the variable or  a function to see the results.  You can cycle through your code to simulate mapping through an array to determine if you see your expected data.  This process is also good to determine if your datatypes are what you expect:  an object or an array, etc.


**REACT/REDUX DEV TOOLS**

The final piece of the debugging process, are the React/Redux Development Tools.

When using React Development tools you can see the flow of your components and see the props available to each component.  If you don't see the expected props, go back and make sure you have passed the props correctly either through the component itself or through the route associated with that component.
 
The Redux dev tools are the top layer of the debugging tree as the Redux state is global.  If your data is not here, it is not available to any of your components.  How many times have your put together your code, created a form entered data, and then not see your form display the data until after you refreshed your browser.  In 99% of the time that means your not getting the data into the Redux store, which means there is a problem with your action and/or the reducer.  Resolve these pieces of the puzzle and you can be assured, your Redux store will update.


**CONCLUSION**

There you have it.  While coding a complete App backend to frontend is quite a task, a Developer's debugging skills are just as important as the skills to coding the App -- you can confirm that (1) your data is correct, (2) the relationships among models are valid, (3) what's rendered is consistent with the backend, (4) your props are available as you need them, and (5) the Redux store holds the data you require to render.




