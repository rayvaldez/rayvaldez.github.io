---
layout: post
title:      "Sinatra basics "
date:       2018-12-20 19:16:30 +0000
permalink:  sinatra_basics
---


Sinatra is a lightweight framework, that is implemented in Ruby to create web applications.  It's simplicity, flexibility, and speed is what makes it a very popular tool to basically take your Ruby code and produce dynamic web applications, websites, web services and web resources.

It essentially focuses front-end and back-end development using the 'Model, View, Controller' framework. Using the common and easiest analogy to break down this idea, we can imagine a restaurant, that contains three different components.

**The Menu/View**
This is what the user will see. It is HTML and CSS combined with Ruby code in .erb files (embedded ruby).  Here we will code a 'menu' of options, which will allow the customer/user to interact with the application.

**The Waiter/Controller**
This is the middleman, that takes orders from the customer and decides what to do with it.  Consider there are a few routes a waiter can relay this order to. A customer might just want a few drinks, or maybe a full on meal. A waiter will decide whether to go to the bar and get drinks, or go to the kitchen and see the chef.

**The Chef/Model**
The chef contains all the logic of the application. It will usually interact with a database, and contain methods to process information.  So in this instance, the chef will use his knowledge of ingredients, and his learned methods to create the dish for the customer.  We could imagine a customer has given specific instructions to remove a particular ingredient, the great thing about Sinatra, is that  we can program in some methods that will allow us to be dynamic with the data. Once the dish has been created he will then send it back to the user via the waiter, who will present it back to the customer.
