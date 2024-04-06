+++
title = "Hinter Wars - Java Client"
date = "2024-04-02T10:33:01-05:00"
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

Already I'm regretting my decision to document this project. 

### The desktop client 

Here it is, in all it's beautiful 800x600 glory. 

![v1.4LoadingScreen](/posts/hinterwars/hinter-wars-java-client/v1.4LoadingScreen.png)


Version : 1.4

Release Date: 2006-11-15

Download: [link](https://archive.org/details/hinterwars)

#### System Requirements 

- Windows(R) XP Platform
- 1.5GHz CPU or equivalent
- 256MB RAM
- 32MB Video RAM
- 190MB free hard disk space for installation
- 200MB free hard disk space for playing
- 16-bit sound card
- Internet connection is required (Broadband connection is recommended)
- Keyboard and mouse


Originally the desktop version of the client was only distributed for Windows, and required the Java runtime, though this is not listed in the system requirements or on the hinterwars website. This means that any system that can run java, can also run this. Though this may not apply to java cards, but who knows, if we can get doom to run on a printer, maybe we can get this run on a card.  

### Oddities

The version of the client which is available on archive.org has an issue where no sounds are loaded regardless of platform. This may be due to an actual development issue, or due to a change in how java deals with sounds. 

While the min specs above state that 190MB's of free disk space, the jar that was distributed is 194MB's plus an additional MB of other files that are used for app removal and initial startup. This does not include the requirement that the user have the JRE installed. 

You might be asking, what causes this? Well dear reader, it is due to the fact that the resources folder is included twice in the jar, pointing us to a poorly configured build/packaging process.

![v](/posts/hinterwars/hinter-wars-java-client/v1.4DupeResFolder.png)



### Decompilation

If we look at manifest.mf in the jar we find that the our application is built with java version 5.0u6 which was initially released on 2005-12-02, nearly a full year before v1.4 was released. This means that we can run the client with basically any jdk after 1.5, though no guarantees that we can actually do anything with decompiled code. 

 ``` properties
Manifest-Version: 1.0
Ant-Version: Apache Ant 1.6.5
Created-By: 1.5.0_06-b05 (Sun Microsystems Inc.)
Main-Class: MastersOfDestiny
 ```


And when we start to do our decompile, we find... 

![v](/posts/hinterwars/hinter-wars-java-client/v1.4Obfuscated.png)

 
G*d damn it. 


Be right back, hold my beer. 