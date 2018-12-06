---
layout: post
title:      "My mission to develope a Crypto CLI Application "
date:       2018-12-06 14:59:37 -0500
permalink:  my_mission_to_develope_a_crypto_cli_application
---


If you ever ask yourself why do I alway have to go to a website to always get updated on the current crypto prices you are definitly not alone. Since I am always using my terminal why not access my favorate site via that interface(for the perposes of this blog lets say it is coinmarketcap.com - will explain more later).

I decided to create a CLI to be able to look up the coin prices in my turminal (because I am way to lazy to type https://www.binance.com/en :P). 

Which was the genius of this project.

## So the over all plan for this project is as follows:
> 1. Make a CLI that gets scraped data from coinmarketcap.com
> 2. Populate the terminal with up to date information about your favorate crypto assets
> 3. Enable the user to be able to choose from the top 100 crypto assets

The interface of the CLI is an important aspect of the application's user experiences. What should the user see and in what format should the data be displayed in. In the beginning of the design phase of the application's development I decided to have a few attributes that I want to follow they are the coin's name, price, 24 hour change, market cap.

I had to change this design for a multitude of reasons but the main reason for the change is due to the website that I initally wanted to scrape (https://www.binance.com/en). I was faced with an issue in regards to the dynmic nature of the site specifically the data that I wanted to scrape - the price, which is in constent flux.

Which lead to the dession to move to coinmarketcap.com (https://coinmarketcap.com/currencies/volume/monthly/ to be exact) mainly to scrape - the name,  symbol, Volume in one day, the volume in 7 days, and volume in 30 days of a coin. The scraped data mainly the volume data is less volitile meaning instead of having the price data changing every second which is expected in this market it is a lot quicker to scrape volume data that changes at minimum day to day. That turned out to be a better approach to enable me ample time to refactor my code within the three day window that I put on myself.

## What does v.1 of the application do now?
When a user runs the program it welcomes you and it displays the top 100 crypto assets with its associted symbols then the user is prompted to enter a number to be able to get more information on their crypto assets. Thats it!

So when you are programming along using Vim ( <3 ) and want to see the volume of your favorate coin you can simply start up a new teminal window and run ./bin/the-crypto-update then whala! you got your very own crypro updater cli application!!!!


## Whats next!? 
Well I plan on adding more bells and wisels like adding a sound when someone want to see a asset, have a sign in/logout functionality to enable for each new user to store the coin they want to check up on. 

If you like this project and want to use this yourself or want to improve on it head to: https://github.com/mfeinLearn/the_crypto_update 

Happy coding!!!








