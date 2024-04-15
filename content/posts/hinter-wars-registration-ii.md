+++
title = "Hinter Wars - Registration II"
date = "2024-04-07T20:55:01-05:00"
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


Let's start designing our "new" registration process / schema, this will help us to remove anything registration related from the client application, and hopefully cleanup a boatload of very very messy code. 


Please note that this is going to be a messy post, mainly notes and stream of consciousness ramblings. 



## What data do we need ?

Our current implementation requires:
1. First Name
2. Last Name
3. Email Address
4. Password
5. Gamertag


Since this is going to be a world class groundbreaking MMO, let's take a page from Blizzard and look at what they're using nowadays: 

1. Country of residence
2. Date Of Birth
3. First Name
4. Last Name
5. Email Address
6. Password
7. GamerTag
8. Acceptance of terms of use (This one isn't really a datapoint, but is part of their flow.)


Well shit, not much has changed in 20 fucking years. Other than the fact that I'm no longer in high school. 




Let's break this down and see what we need and what would be nice to have. 


|Field|Necessary?|Reasoning|
|--|--|--|
| Country Of Residence | yes | Helps us to determine if we need to gather additional demographics information. laws and stuff |
| Date Of Birth | Yes | Users must be at least 13, if not older depending on country. |
| Name (first and last) | Maybe |  Do certain countries require this ?   | 
| Email Address | Yes |  We could go solely with a gamertag, but that would allow for spam signups, which would be a problem. Fucking bots.  |
| Password | YES, YES, DEAR GOD YES | Since we aren't outsourcing our SSO to another identity provider. |
| GamerTag | For Sure |  |
| Terms Of Use | Totally |   |



#### Laws and such

Due to the fact that there are more country's than just America, despite the cultural hegemony it seems to still have, we will need to be able to configure the server to require more data depending on the country of residence of the individual we are registering . Honestly I dont like this, but we might as well try to be as realistic as possible. Instead of asking the user where they live, because people do tend to lie, we are going to do some geofencing in order to determine if we require a name, and also in order to determine the minimum age at which the user can have an account. All of this is going to be determined server side, and be configurable by system administrators, mainly due to laziness. 


### So how are we doing this. 

Due to the fact that everything about this game is meant to be as affordable as possible, we are going to going to use a self hosted install of [Authentik](https://goauthentik.io) and run it in a set of docker containers. 

#### Pages

1. Email
2. Demographics
    1. country (prefilled based off of ip address)
    2. first name
    3. last name
    4. age 
3. UserId/Gamertag (this must be unique)
4. Terms of service
5. Review
6. Confirmation / Go check your email


#### Rules/logic

1. We do not check to make sure the email is unique until we are just about to do final submission this way we don't leak info about already registered users.
2. If the user selects a "restricted" country or if their ip address shows them to be in one, then the process exits. We do give them the benefit of the doubt and allow them to select from the dropdown.  
3. If the user selects an age that is below the min set by the selected country, or is below the system configured minimum then we exit. 
4. UserId/GamerTag must be unique (what characters are we limiting the tag to ? who knows?)
5. Need a "search" method in order to check usernames, additionally will want a "suggested/related" and "random" username service
6. If the email provided is unique then we send a verification email, if it isn't unique then we send a "hey someone tried to register, was it you?" email.


