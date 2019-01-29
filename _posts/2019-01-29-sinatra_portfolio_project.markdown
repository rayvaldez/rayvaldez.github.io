---
layout: post
title:      "Sinatra Portfolio Project"
date:       2019-01-29 18:49:25 +0000
permalink:  sinatra_portfolio_project
---

![](https://laligaexpert.com/wp-content/uploads/2019/01/Screenshot-2019-01-03-at-19.44.49-1.png)


### Why football index?
Football Index is essentially a mix of fantasy football and a stock market platform, where you can invest real money in shares (or 'futures' as it is referenced) and watch as a players value rises or falls depending on performance and media attention. Because the company is still very much in its infancy, the website provides only basic information on the players you invest in, so I proceeded to create a spreadsheet for the purpose of tracking my investments.  With that in mind I thought it would be useful to recreate this spreadsheet into an application, and Sinatra with ActiveRecord would be the perfect way to do this.


### Overview of Application
I have two models, a **User** and **Player**, with the relationship being that a **User** *has_many* **Players**, and a **Player** *belongs_to* a **User**.

A **User** will create an account, with 3 attributes: A username, email address and password. 

They then can proceed to create a **Player** using data that is provided on the website, that being the **Player** name, Team name, total cost of futures and quantity bought.

The show page for each **Player** displays a bit more information on the particular **Player**, and allows the **User** to either edit a **Players** details or delete the **Player**.


### Coding the App
Getting started was fairly straightforward (once Corneal finally decided to work on my laptop).  I managed to create my models and associations aswell as map out all of the RESTful routes without any hiccups.  I allowed for Users to sign up or login using the **BCrypt** gem, and created a session for each User.

I added a navigation bar to the top of the page for easier direction through the application.

As there would be no shared views for each user, I added code which would iterate through all created **Players** and only show those that were created by the current **User**. I found the helper methods very useful to allow this.

The show page is intended to go into detail for a single **Player**. I have only added an extra column for now, but I know I can extend this feature in the future.

After all the RESTful routes were created I proceeded to add validation to the forms. Some in the form of Rack Flash messages, and others with the help of some regex and javascript.  This was so that a User would not be able to input bad data.

Next was to redirect any **User** from viewing pages if they were not logged in, and finally I added a few CSS properties to make the website better to view.



