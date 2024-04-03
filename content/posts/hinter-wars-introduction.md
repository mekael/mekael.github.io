+++
title = "Hinter Wars : An Introduction"
date = "2024-04-01T20:55:01-05:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["HinterWars"]
keywords = ["", "HinterWars"]
description = ""
showFullContent = true
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++


It's been some time since this thing's been updated, and while a lot has happened, nothing much has really changed.  So back to reverse engineering we go! The next couple of months will be an interuption to our "build yourself a new yorker" one, mainly because I keep getting distracted / bored / stuck in analysis paralysis. 



### Previously scheduled programming


Introducing "Hinter Wars 2 : The Hintering", a series where we take an obscure mid 2000's MMO which never truly launched, and rebuild it into something that can actually be played. 

Announced on [March 5th, 2005](https://web.archive.org/web/20050729075512/http://www.hinterwars.com/html/news&countdown.html) Hinter Wars:The Aterian Invasion was an MMO developed jointly by Nokia and Activate Interactive designed to be played both on the Nokie NGage and on the PC via a java based client application. The game shared one account between all devices, so that a player could seamlessly transition from PC to N-Gage without losing any character progression, a feature which still hasn't been implemented in some MMO's today (looking at you warframe). 

But it seems that  the trainwreck that was the N-Gage platform also took out any hopes of Hinter Wars becoming the next WOW, as the service seems to have been left to rot, with the last  update to the client (1.4) coming in the middle of November 2006.  There is no exact listed date of death for the game, but it lasted through at least the first part of 2007 as there is [mention](https://twitter.com/pocketsuke/status/1671926573811875843) of a tournament in Indonesia in January of that year. 

The last official post on the forum looks to be in April of 2007 announcing that the [support email would be offline](https://web.archive.org/web/20090508140305/http://www.hinterwars.com:80/forums/viewtopic.php?p=1637) . Shortly after this the forum would start to be filled with spam for ***NSFW*** [adult websites and products](https://web.archive.org/web/20080215110634/http://www.hinterwars.com/forums/viewforum.php?f=10&sid=2eec1f6b3ca4b6afb4a8430dd83680f1)
 

 ## Gameplay

Due to the fact that the internet is not truly forever, there are few, if any, videos of actual gameplay. Some trailers do exist such as this one for the [Season 2 trailer](https://www.youtube.com/watch?v=rriTa7ZVUGA), which seems to point to a continuation of the series, though there is no actual date as to when the video was produced. This [video](https://www.youtube.com/watch?v=067kb0Fwjlc) looks to be the original trailer for the game, but it is very light on content other than a pre rendered scene for each of the four races that players can chose from. 
There do exists some screenshots [here](https://www.unseen64.net/2022/02/10/hinter-wars-aterian-invasion-ngage-pc-cancelled/) which are from mainly from the N-Gage version of the game.  


I'm going to end this here, getting tired of typing. 


### F.A.Q.

***Q: Why are you doing this?***

Because as of late i've been working on reverse engineering some old XBLIG games (posts to follow later), and it was a fun time for the most part. The largest hurdle was always the networking portion, xblig support was removed years ago, and it fascinated me that there was no easy way to get something setup for each game. There is a steam replacement which can be used in place, but that does make it hard to do testing. This is my attempt to do a full end to end project. 

***Where did you find this game?***

I routinely search archive.org for different things to reverse engineer or things that are historically interesting. This happened to show up, and after a bit of research found that the client was easily disassembled, which should aid in the effort 


***How long is this going to take?***

Probably six to eight months. Or like a decade.  

***Q: Is this a bit?***

Why wouldn't it be ? The whole world is a bit. 

***Q: Will this be a faithul recreation of mid 2000's gaming?***
The hope is that we can successfully reverse engineer the server from the two clients that are available, but most likely there is a lot of logic that we will have to guess at. The rules for quests, some of the skills stuff.  We may or may not implement the chat feature, mostly because that's bound to be be abused. 

***What will you be updating?***
Given the era in which this game was written, security was probably one of the last things on the developers minds, so anywhere that we can we will be adding in security measures such as anti-cheat and tls. Screen resolution is still at 800x600, so w
