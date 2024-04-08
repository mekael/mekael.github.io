+++
title = "Hinter Wars - Registration"
date = "2024-04-06T20:55:01-05:00"
author = ""
authorTwitter = "" #do not include @
cover = ""
tags = ["HinterWars"]
keywords = ["", "HinterWars"]
description = ""
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++


After a massive amount of cleanup we are finally ready to start start dissecting the code around new user registration.


![cats](/posts/hinterwars/hinter-wars-registration/RegistrationScreenshot.png)


## School Registration 


This screen is made up of several components all of which are hard coded to fit inside the 800x600 window.  Did I mention that this entire game doesn't really support scaling ? 

- Background image with static fields (see below)
- Code generated fields superimposed over the background image
    - 3 text fields 
        - First Name
        - Last Name
        - Email Address
    - 2 password fields
        - Password
        - Reenter Password. 


![Background Image](/posts/hinterwars/hinter-wars-registration/register_main.png)



Form Validation steps
1. Check that all fields have a value
2. Activate the "Register" button
3. Upon clicking register we check to make sure 
    1. Password and ReenterPassword match
    2. Password is less than or equal to 10 characters
    3. UserName is less than or equal to 10 characters. 


Once we pass all of the checks for registration we can finally start communicating with the server! 


1. open socket and connect to server
2. wait for a "CS" transaction from server (Connect to Server)
3. Send VC transaction with the client version to the server
4. if the response is WV then we have an old version then we tell the user to update
5. if the response is VC (version check passed) then we send our registration
6. registation is sent in 4 parts each of which is terminated by a newline
    1. "R1|FirstName|LastName"
    2. "R2|EmailAddress"
    3. "R3|UserId|Password"
    4. "R4"
7. if the server responds with an "R6" then there was some sort of error and the appropriate message is displayed based on the error code. 
    1. the actual response is "R6|ErrorCode"
8. If the registration was successful then display the "success" message and set the game state to 10. 

### Thoughts

There are several issues with the registration process:

1. Password length is abysmal at 10
2. Username possibilities are also abysmal due to the max length of the 
3. Existing user emails are leaked.
4. Registration is immediate and does not seem to require email verification
5. Web based registration is currently disabled, and may not have been available at launch. 
6. No reset password link


When registration is reimplemented we want to :

1. Require a stronger password, and check passwords against known bad password lists
2. Suggest alternative usernames if the one chosen is taken. 
3. Instead of say "Success" tell the user to check their email 
    1. If the email address hasn't already been used then send a "someone tried to register again, was it you?"
    2. If the email hasn't been used then send them a "click this link to complete registration"
