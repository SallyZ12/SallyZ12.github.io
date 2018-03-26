---
layout: post
title:      "My Sinatra Project: Credit List"
date:       2018-03-26 11:48:50 -0400
permalink:  my_sinatra_project_credit_list
---


Of all the things I could have picked for my Sinatra Project, everyday life occurrences, sports involvement, etc. I ended up creating an app that goes back to my days in Public Finance-- spending my career in the Bond Insurance Industry.

While in my career I worked with developers to craft internal online systems that tracked the lifeblood of the industry--which credits were in the portfolio and which transactions were approved to provide credit enhancement, I never had the opportunity to see just what the developers did behind the scenes so that the database accurately tracked the business and provided the validations I always demanded so that bad data never found its way into the database.

 Once I was able to create Credits and associate transactions for each User, as well as display all Credits and all Transactions no matter who the user was, the validations became the most meaningful aspect of the app for me.  There is nothing worse than bad data infecting the database.
 
 Some of my rules included, when you create a first time Credit, you must also create a transaction at the same time -- you wouldn't know the Credit unless you had a transaction.  Also, a transaction can't be created unless its associated with a Credit.
 
 My validations included:  
 
 1. Sign-up:  username and/or password missing-- can't sign-up
 
 2. Log in:     username and/or password missing -- can't login


 3. First time Credit:  You must create a transaction at that time.  Alert for missing credit data and alert for missing transaction data.  No Credit or Transaction created.
 
 4. New transaction with an existing Credit: If you do not select an existing Credit, alerted and no new Transction created.
  This was my most challenging validation  since I kept focusing on a radio button selected or not rather than the actual code verifying whether or not the Credit existed as selected.   I split the validation into 2 parts, 1 for the Credit and 1 for the Transaction.
 
 Here is the code that achieved this objective:
 
  
```
  if  params[:transaction][:credit_id] == nil
          flash[:message] = "You Must Select a Credit"
            redirect '/transactions/new'
          else

          if params[:transaction][:name]== "" || params[:transaction][:series] == ""
              flash[:message] = "You Must Enter Transaction Information"
                redirect '/transactions/new'
 
```
 
 
 5. Delete Credit:  If attempt to delete a Credit that has associated Transactions, you will be stopped and told to delete the transactions first before a Credit can be deleted.

Here is the code that achieved this objective:

```
if !@credit.transactions.ids.empty?
          flash[:message] = "To Delete A Credit You Must Delete the linked Transactions First! Select User Credits Only"
          redirect "/credits/#{@credit.id}"
```

At the end of the day, this app is very basic, but it taught me about the integration of the database, models, controllers, views, and browser.  Data in via a form through a  route and data manipulation within that action and data out to another form is key to a succesful app.

In conclusion, an app is only as good as its data!





 
 
