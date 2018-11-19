---
layout: post
title:      "CLI Gem : FifaTop100"
date:       2018-11-19 21:12:20 +0000
permalink:  cli_gem_fifatop100
---


So as per my last blog, I have finalised the first draft of my gem.

My gem is aimed at Football (UK) fans, more specifically Fifa gamers, that can find information on the highest rated players as of 2019.

[Fifa 19 Top 100 Rated players](http://www.easports.com/fifa/ratings/fifa-19-player-ratings-top-100#10-1)

To break it down, I first created a #Scraper class, the purpose was to iterate over each rated player, and grab 4 elements, a players name, rank, team and short description, and place them into a hash.

I then created a #Player class that passes in data from the scraper class as an argument, and instantiates a new player with these attributes and saves them into an array.

The CLI class then uses the data from that array and displays the desired information to the user.

**# *Overcoming obstacles***

With careful and thorough planning (3 days worth of watching and googling)  I managed to code the gem pretty quickly, but one problem that put a spanner in the works, and literally cost me a whole days worth of time, was that I realised the website displayed the list of players in an unconventional way:

> 10 -1
> 20 -11
> 30-21
> 40-31
> 60-41
> 80-61
> 100-81

This would mean that my array full of players would go from 10 to 1, and then from 20 to 11 and so forth. Not ideal when you need it all in order. Pondering all the options, I thought the first thing to do would be to scrape each list separately to get it ordered, but was immediately shut down by the requirements of the project: not to scrape data more than once.

I knew I had the perfect attribute to sort the array, :rank, but it took a while for me to get the code working. 

```
@@all.sort! { |a, b| a.rank.to_i <=> b.rank.to_i }
```

Finally my list was now displaying in order, and I can now get over the shame of overlooking something so simple!

***# Conclusion***

Preparing/planning is the biggest advice I can give someone who is currently doing this project. I wrote a book load of notes, and watched a days worth of tutorials and googled plenty. I was very apprehensive to start, but the more I prepared, the more excited I became. Understanding how to set up your environment, the interplay between namespacing classes and require_relative has made me much more better than how I was at the start.

