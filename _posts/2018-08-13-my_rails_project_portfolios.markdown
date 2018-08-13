---
layout: post
title:      "My Rails Project: Portfolios"
date:       2018-08-13 21:23:33 +0000
permalink:  my_rails_project_portfolios
---



As I reached the time in the curriculum to begin my Rails Project, I thought about how best to design my way into a Ruby on Rails Application.  Before even starting any coding, I did lots of organizing using an excel spreadsheet and drawing many diagrams.  I wanted to make sure I understood what I wanted to accomplish and the requirements of each step.  I defined each model, each attribute for each table, the associations between models, and what Class Scope Methods I wanted to include.  It is at that point that I started to code.

The following is my Project Structure:

1. Models: User (Company), Credit, Exposure, Transaction
2. Tables: users, credits, exposures, transactions
3. Join Table: Exposures - joins users and credits and  includes attributes of limit and rating.
4. Associations:
         User: has_many exposures, has_many credits through exposures, has_many transactions through exposures
				 
				 Credit: has_many exposures, has_many users through exposures, has_many transactions through exposures
				 
				 Exposure: belongs_to user, belongs_to credit, has_many transactions
				 
				 Transaction: belongs_to exposure
				 
5. Validations: as there are numerous, almost all attributes on each table has some kind of validation, plus macro validations defined within the controller or views. 
6. Secure Password:  I used the bcrypt gem and the "has_secure_password" method in  my User model along with the password_digest attribute to ensure a secure password .  
7. For third-party logins, I used  omniauth, omiauth-github, and dotenv-rails gems and ran a local secure server using the thin gem.

The goal of this Project is to allow various Companies (Users) to add Credits and related Transactions to their portfolio.  The combination of a Company and a Credit is defined as an Exposure.  For each Exposure a limit is defined by the Company and when the aggregation of Transaction Par for that Credit exceeds the Exposure limit a violation occurs and is displayed as a "Yes" in the Violation column of that Exposure.  There are also 2 different Rating categories (1) Company Rating which is an internal rating specific to an Exposure, and (2) External Rating which is a third party rating assigned to a Credit (such as from a Rating Agency).  The External Rating must be assigned at the time a Credit is created.  The Company Rating is assigned at some point in the Transaction approval process (which is not part of the scope of this Project) and therefore, is not required to be assigned at the time the Exposure is created.

To accomplish many of these Project goals, various helpers and partials were used to streamline the code in views.

For example, in creating the Class Scope Method for selecting Credits by a specific state, the drop-down selection screen was placed in credits_helper:

```
module CreditsHelper

def select_state
    select(:credit, :state, Credit::CREDIT_STATE)
  end
	
end
```
This method was then used in the Credit Model Class scope as follows:

```
  scope :pick_state, -> (select_state){where('state = ?', select_state)}

```
Another helper was defined to allow me to display whether the sum of Transaction par in an Exposure exceeded the limit.

```
module ExposuresHelper

  def display_violations(exposure)
    exposure.limit - exposure.t_sum < 0 ? "YES" : "No"
  end

end

```
This helper was then used in the Exposures show view as part of the table.

```
<%= display_violations(@exposure) %>
```


Another important feature of the application is keeping it "DRY".   At first, the Rating scale was defined as a Class Constant in the Credit Model.  However, when I decided to add another Rating input for Exposure, it didn't seem "DRY" to define the Rating Scale in the Exposure Model as well ---that is the beauty of using the Application_Record Model.  I defined the Rating Scale as a Class Constant in the Application_Record Model and both the Exposure and Credit Models inherent from the Application_Record Model.  Therefore, the Rating Scale exists in one place and any modifications   occur only there.

Finally, any good application needs to prevent unauthorized users from executing certain functions or preventing certain actions from occurring that would result in corrupted data or a broken structure, to name a few.  I've added a number of these validations to my controller.

For example, in the Transaction's Controller delete action, a User is prohibited from deleting a Transaction that is not part of their own Portfolio.  This is accomplished by defining who the current_user is (the Application Controller) and displaying the action -- success when the Company deletes its own Transaction or -- failure:  "You are not authorized to delete this Transaction".

```
def destroy
    @transaction = set_transaction
      @exposure = Exposure.find(params[:exposure_id])

        if current_user.id == @transaction.exposure.user_id
          @transaction.destroy
          flash[:message] = "Transaction Successfully Deleted"
    redirect_to exposure_path(@exposure)
        else
          flash[:message] = "You Are Not Authorized to Delete This Transaction"
    redirect_to exposure_path(@exposure)
    end
  end
```
Another way of imposing this kind of control is by not having the ability to edit or update any data from a different user's Portfolio.  This was accomplished with the form helper "link_to_if" in my Exposures Show page.

```
<%= link_to_if(current_user.id == @exposure.user_id,  "Edit Exposure", edit_exposure_path){} %>
```
Once again the current_user method comes in handy.  My condition is if the Company is the same one defined in the Exposure (remember, the exposure is the join table and contains the foreign keys - user_id and credit_id) then you have full CRUD capabilities, otherwise, you will not even see the link for those actions.

While all the above is a bunch of disparate ideas and functions, after working through this Project I can say that I learned as much about how to build this app from the failures I encountered and the quest to resolve those failures.  The "MVC" structure of a Rails app provides the foundation for you to achieve your goal.  However, only by testing your code and failing and refactoring and failing again and then refactoring to streamline your code once it finally works do you feel like you have really accomplished your goals.  So for now, I end this blog with the satisfaction that while it took me weeks to accomplish due to a variety of outside forces working against my schedule, I have come to the end with a greater understanding of not only Rails, but Ruby as well--so I guess you can say I've begun to appreciate "Ruby on Rails".












