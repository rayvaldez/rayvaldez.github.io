---
layout: post
title:      "Javascript Portfolio Project!"
date:       2020-10-19 18:39:43 +0000
permalink:  javascript_portfolio_project
---


Time seriously does fly. One baby and a great deal of time later I have completed my fourth portfolio project.

## What problem did my project solve?

More of a personal gripe than a nobel prize winning solution. Following on from my Sinatra portfolio project, I have continued to work with the www.footballindex.com website in mind, a stockmarket where you can buy and sell shares of football players. The company is still in its infancy, and I saw an opportunity to code my own watchlist application, which at the time of creation, wasn't available on the actual website (but has been implemented since...oh well!).

The website does not allow for an API to be used just yet. So my application is a hypothetical example of how I would code and design it, complete with a method to randomly change the current price of footballers to mimic movement in a stockmarket.

## Implementation

### Setting up the API

I have two models:

* **Players** - These are the players available on the market. The player will have a name, a team and a current price.
* **Watchlist Players** - This will be the watchlist of added players. The watchlist player will have a unique player ID to catch any duplicate entries, cost (price when added), name and team.

I added seed data to display the top 50 players currently on the Market, and I used the *active_model_serializers* gem to serialize the JSON.

In the Players Controller I added the following method: 
```
  def update_players
    players = Player.all

    players.each do |player|
      if Time.now - player.updated_at > 60
        player.cost = player.cost * rand(0.99..1.01)
        player.save
      end
    end
  end
```

I declare the variable players to equal all players and then iterate through each instance of a player. The next line of code checks to see if the time now has surpassed 60 seconds since it was last updated. If so then the cost attribute of the player is multplied by a random number between 0.99 to 1.01, this will give small movement in price, and then the instance of the player is saved. This is added as a filter method in the Players controller, and will be executed each time the Index route is requested.

### Frontend

There are 2 tables on index.html and when the view is loaded, a fetch method is triggered to obtain the players from the API and populate the players table.  I have attached event listeners to each Player, which when clicked, will bring up a modal. Inside this modal I have placed hidden input values which upon submission, will be the data used to **POST** to the back end and create a new Watchlist Player.

I added a button next to each Watchlist Player to Update/Remove the player in question and when clicked, I have again added hidden input values. If the Update button is clicked, a hidden 'current cost'  value is used to **PATCH** this data to the backend. If the Remove button is clicked, the 'watchlist player id' is obtained from the hidden input value to find the player and **DELETE**  them from the API and consequently the table.

## Conclusion

I definitely enjoyed the beginning and end of coding this app. However, working out the correct syntax and code in some of the functions did set me back a few hours, days and even weeks. Overall it was really satisfying to actually get to come up with my own engineered solutions to make issues easier. I am looking forward to problem solve more issues, with more tools under my belt!
