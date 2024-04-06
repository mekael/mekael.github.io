+++
title = "Hinter Wars - Java Client II"
date = "2024-04-04T10:33:01-05:00"
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


In our previous episode we found that our neat and tidy code base is actually a giant tire fire of obfuscated code. 

Luckily we have the engineers best friend.

### Print Statements

Looking through the codebase we find references to a static function which is passed strings that look like debug statements. 

``` java
package defpackage;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Hashtable;

/* renamed from: al  reason: default package and case insensitive filesystem */
/* loaded from: hinterwars.jar:al.class */
public class C0012al {
    private static C0012al a;
    private Hashtable b = new Hashtable();
    private String c;

    public C0012al(String str) {
        this.c = str;
    }

    public static C0012al a(String str) {
        if (a == null) {
            a = new C0012al(str);
        }
        return a;
    }

    public String a(long j) {
        String str = (String) this.b.get(Long.valueOf(j));
        if (str == null) {
            try {
                InputStream resourceAsStream = getClass().getResourceAsStream(this.c + j + ".txt");
                if (resourceAsStream != null) {
                    ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
                    while (true) {
                        int read = resourceAsStream.read();
                        if (read == -1) {
                            break;
                        }
                        byteArrayOutputStream.write(read);
                    }
                    resourceAsStream.close();
                    str = new String(byteArrayOutputStream.toByteArray(), "utf-8");
                }
            } catch (IOException e) {
            }
            if (str != null) {
                this.b.put(Long.valueOf(j), str);
            } else {
                //Here is our new best friend
                C0032k.a("MonsterDetailsManager.getMonsterDetails: Error! - Problem loading monster details string");
            }
        }
        return str;
    }
}
```


If we look at that function we find that it doesn't actually do anything at the moment, but can be taken advantage of

``` java
package defpackage;

/* renamed from: k  reason: default package and case insensitive filesystem */
/* loaded from: hinterwars.jar:k.class */
public class C0032k {
    private static final boolean a = false;

    public static void a(Object obj) { }

    public static void a(int i) { }

    public static void a(long j) { }
}
```

Clean up the class name, add our println and we're off to the races.  

``` java
public class Logger {
    private static final boolean a = false;

    public static void Log(Object obj) { System.out.println(obj.toString());  }

    public static void Log(int i) { System.out.println(i); }

    public static void Log(long j) { System.out.println(j); }
}
```



Over a period of two or three days we sort through all of the existing logging and can begin to discern what some of our application looks like, and how we can start to reverse engineer the server.  And so we begin to get a few classes that look something like :

``` java
package com.alsocats;

import java.io.ByteArrayOutputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Hashtable;

/* renamed from: al  reason: default package and case insensitive filesystem */
/* loaded from: hinterwars.jar:al.class */
public class MonsterDetailsManager {
    private static MonsterDetailsManager monsterDetailsManager;
    private Hashtable monsterDetailsDictionary = new Hashtable();
    private String folderPathToMonsterDetails;

    public MonsterDetailsManager(String folderPathToMonsterDetails) {
        this.folderPathToMonsterDetails = folderPathToMonsterDetails;
    }

    public static MonsterDetailsManager getMonsterDetailsManagerInstance(String folderPathToMonsterDetails) {
        if (monsterDetailsManager == null) {
            monsterDetailsManager = new MonsterDetailsManager(folderPathToMonsterDetails);
        }
        return monsterDetailsManager;
    }

    public String getMonsterDetails(long monsterId) {
        String monsterDetails = (String) this.monsterDetailsDictionary.get(Long.valueOf(monsterId));
        if (monsterDetails == null) {
            try {
                InputStream resourceAsStream = getClass().getResourceAsStream(this.folderPathToMonsterDetails + monsterId + ".txt");
                if (resourceAsStream != null) {
                    ByteArrayOutputStream byteArrayOutputStream = new ByteArrayOutputStream();
                    while (true) {
                        int read = resourceAsStream.read();
                        if (read == -1) {
                            break;
                        }
                        byteArrayOutputStream.write(read);
                    }
                    resourceAsStream.close();
                    monsterDetails = new String(byteArrayOutputStream.toByteArray(), "utf-8");
                }
            }
            catch (IOException e) {
                Logger.Log(String.format("MonsterDetailsManager.getMonsterDetails: Error! - Problem loading monster details for monsterId "+monsterId+" :  "+ e.getMessage()));
            }
            if (monsterDetails != null) {
                this.monsterDetailsDictionary.put(Long.valueOf(monsterId), monsterDetails);
            } else {
                Logger.Log("MonsterDetailsManager.getMonsterDetails: Error! - Problem loading monster details string");
            }
        }
        return monsterDetails;
    }
}

``` 

After a lot of cleanup and fixing some issues where our initial decompile  had unreachable code due to infinite loops we get ... 


[v1.4LoadingScreen](/static/posts/hinterwars/hinter-wars-java-client-ii/v2.0Recompile.mkv)



Hooray!  Now time for a well deserved nap.  Next time we can talk about some of the design of the application