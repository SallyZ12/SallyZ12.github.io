---
layout: post
title:      "Rails/JS Project: JS Static Form vs Rails Form_For"
date:       2019-03-09 21:31:03 +0000
permalink:  rails_js_project_js_static_form_vs_rails_form_for
---


Sometimes simple is best, other times it's just easier, but other times the journey to make something simple, is just not worth it.  My project structure is complicated.  There are 4 models: User, Exposure, Credit and Transaction with many relationships and a variety of attributes that are entered either with dropdown menus or just plain text.  As I'm using "secure_password" to encrypt my passwords, a must in this need to keep "data private and secure" world in which we live, I had not realized that creating a simple input form in js would present problems I had not anticipated at the start.

My simplest model is my Transaction, which belongs to an Exposure and in which an Exposure has many transactions.  Exposure on the other hand, is  a join table of User and Credit, with a few additional attributes.  Since transactions only have 3 simple attributes of name, series, and par (a dollar amount), I thought I would stretch my skills and create a static form in my transaction.js file. 

I know that when I created an AJAX response to create a new Credit object, I (1) created a partial file ("ajax_new.html.erb") in my credit view using "form_for" to create the form (which includes several attributes selected from drop-down menus), and (2) in my Credit Controller new action added a render to this partial.  As part of my postCredit(), I declared a variable, "inputs":


       let inputs = $(this).serialize();
			 
	 
The variable "inputs" is the serialised version ("string") of the credit object's attributes: credit_name, credit_sector, credit_rating, and credit_state  all the key fields of my "form_for", including 2 key header variables; (1) utf8 and (2) authenticity_token.  That is the magic of using the Rails "form_for"  with a partial.

The hijacking of my credit form, entering my data, and then displaying the response in my browser (client) worked seamlessly.


Then the challenge began.  I though to myself, why not try to use a static function and create a form directly in a js file -- let's skip the Rails partial using "form_for".  I attempted this methodology  using my Transaction model since  a new transaction required only 3 attributes, all populated by text.  I created a static function for my  form in my transaction.js file.

```
static newTransactionForm() {
    return (`
      <strong> New Transaction Form </strong>
      <form id="new-transaction"> <br>

        Name: <input type='text' name= "name"> <br>
        Series: <input type='text' name="series"> <br>
        Par: <input type='text' name="par"><br>

        <input type='submit' value = "Create AJAX
         Transaction">
      </form>
      `)
    }
};
```

I thought to myself, wow, this was easy.  I had everything working, a function to hijack the screen (listenForNewTransactionFormClick), a function to create a new transaction (postTransaction), and finally,  the form was rendered to the browser as anticipated.  Now the real test, what would happen when I entered data in my 3 fields: name, series and par and selected "submit".

Well, nothing happened.  I did not get anything rendered to the browser.  I checked my code.  looked for typos, checked scope, format issues, did I correctly identify the ids, etc.-- nothing!  So, I checked the server.  I kept seeing an error message each time I attempted to post a new transaction - "param missing".   I compared the "inputs"  variable of my postCredit function with that of the  "inputs" variable of my postTransaction function and the difference was the header values  "utf8" and "authenticity_token".

I researched ways to get these header values included in my transaction form.  It was a bust.  Then I realized, I don't want those values in my  transaction "inputs' variable.  Those values would be exposed in the browser.  The sole reason of using a secure_password was to protect my data with a high level of security.  If I solved my quest to get those values in my data headers for the js form, then I would be winning the battle, but losing the war.

Sometimes when you attempt something and fail, the process of the attempt is where you learn the most.  It took me many hours and failed attempts to figure out why my rendering of a new transaction was failing.  I used the Chrome Development tools and when that did not prove productive, I looked at the server messages.  Upon zeroing in on the issue, and failing at my attempt to add utf8 and authenticity_token into my form, I realized there was probably a reason why this quest was difficult.  Exposing those values in the browser is one of those things you should never do.  That is why the battle of "form_for" versus a static js form was won by "form_for", at least when security is a priority and you're not using a simple form with no login or private data.  

I learned an important lesson in this process which also improved my debugging skills.  While this took a chunk of time to come to a resolution, I don't mind as the experience was well worth it.

