+++
title = "Hinter Wars - Host And Port Fun"
date = "2024-04-05T10:33:01-05:00"
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

Now that i've got a tonne of cleanup finalized, let's start to do so research about the game server and how we can reimplement it.  

From our initial code and name cleanup we are able to identify the host and port that were originally used and that we can provide a host/port combination from the command line (that is done in MastersOfDestiny which we have ommited due to the fact that it just passes a string from our initial args ( ) to this constructor).


{{< highlight java  >}}
    public GameStates(MastersOfDestiny mastersOfDestiny, String userSuppliedHostPort) {
        this.ServerPortPrefix = "5";
        this.ServerHostName = "b1.main.hinterwars.com";
        this.entityManager = null;
        this.pZ = false;
        if (userSuppliedHostPort.compareTo(sm) != 0) {
            String[] split = userSuppliedHostPort.split(":");
            String str2 = split[0];
            this.ServerHostName = str2;
            this.ServerPortPrefix = split[1].substring(0, 1);
            Logger.Log("GameStates(Constructor): Server address now set to=> " + str2);
            Logger.Log("GameStates(Constructor): Port now set to=> " + this.ServerPortPrefix);
        }
        SoundManager.soundManager.playSoundEffect(SoundFilePaths.MouseClickSuccessfulClipFilePath);
        pS = mastersOfDestiny;
    }
{{< / highlight >}}

Taking a look at the code above we see that the port number is found by splitting the host:port combo from the command line if available. If you're thinking, is this a direct socket connection rather than http calls, you would be correct.  Take note of the fact that the hardcoded address of b1.main.hinterwars.com:5 seems strange due to the fact that port 5 is generally reserved 

![Wikipedia says it's so](/posts/hinterwars/hinter-wars-host-port-fun/WikipediaPortFive.png)


The system is down, the system is down.

``` 
% 
ping -c 10  b1.main.hinterwars.com
PING b1.main.hinterwars.com (165.21.82.233): 56 data bytes
Request timeout for icmp_seq 0
Request timeout for icmp_seq 1
Request timeout for icmp_seq 2
Request timeout for icmp_seq 3
Request timeout for icmp_seq 4
Request timeout for icmp_seq 5
Request timeout for icmp_seq 6
Request timeout for icmp_seq 7
Request timeout for icmp_seq 8

--- b1.main.hinterwars.com ping statistics ---
10 packets transmitted, 0 packets received, 100.0% packet loss
```



Additionally we take a substring of the user supplied port and only care about the first digit, meaning that we are limited to ports 0 through 9.  Or are we?  Further down in GameStates.java we find the only reference to get the host name and find that whatever port we had decided upon earlier is just a prefix. Now we find that we have access to 10 ports but at least they are outside of a generally reserved range. ports x224 may be application specific but are not in general use. 

```java
//this code actually helps us with our initial login, the game server may have been hosted on an entirely different server/port combo 

    public void g() {
        String str = this.ServerHostName;
        int parseInt = Integer.parseInt(this.ServerPortPrefix + "224");
        try {
            this.pi = 0;
            this.pk = false;
            this.pl = 100L;
            this.pm = System.currentTimeMillis() + this.pl;
            this.pb = new Socket(str, parseInt);
            this.pe = new PrintWriter(this.pb.getOutputStream(), true);
            this.BufferedReader = new BufferedReader(new InputStreamReader(this.pb.getInputStream()));
            this.pc = new Y(this);
            this.pc.start();
        } catch (IOException e) {
            Logger.Log("auth conection error = " + e.getMessage());
            a("Unable to connect to the server.\nPlease try again.\n[Press Any Key]", (byte) 8, (Object) null);
        }
    }
```

For our initial server build we will need to need to pass in the startup server at the command line, but in the future the user should also be able to enter the server in the ui before we start the actual game. 