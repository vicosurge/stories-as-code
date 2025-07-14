---
title: "Compiling NXLog CE 2.10.2150 and the dreaded -lcrypto error"
date: 2020-09-04 21:07:00 -0800
modified: 2024-12-13 21:26:00 -0800
tags: ["NXLog", "Community Edition", "IT", "Cybersecurity", "english"]
author: Vicente Manuel Muñoz Milchorena
---

Just two days was the time I had to mess around with to figure out what the 
problem was, sometimes this is not intuitive but it goes to tell you to be 
careful in what you are doing and what packages you are using to compile.

For whatever reason this worked in CentOS 7, which intrigued and bothered me 
at the same time because I had done this in Debian.

Now you may be wondering, why would you need to compile it in Debian? Because 
I have two Raspberry Pi which I use to capture and move logs around, so right 
now I am intrigued since I know this can be done.

My main issue was an error that mentioned **-lcrypto** not found, *but I have the 
ssl dev package installed, what gives?*

No, turns out I had the wrong package installed and there is no reference to 
this anywhere, *I even compiled OpenSSL from the source code to test this out.*

So to cut this short, this should be the list of packages you need to compile 
this from source in Debian or Ubuntu at the 2.10.2150 version.

If you got any problems with that let me know, also remember that you may need 
to add contrib and non-free to your repositories.

Debian Compilation of NXLog

`apt install build-essential libapr1-dev libpcre3-dev libssl1.0-dev libexpat1-dev`
