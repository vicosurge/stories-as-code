---
title: "Installing OSSEC 3.6.0 on Debian 10 (Buster) How-To"
date: 2021-09-14
tags: ["security", "ossec", "hids", "monitoring", "linux"]
categories: ["Security", "System Administration"]
description: "A comprehensive guide to installing and configuring OSSEC HIDS 3.6.0 for host-based intrusion detection"
author: "Vicente Manuel Muñoz Milchorena"
---

## Installation

Because this always catches me by surprise when I need to do it, and while I
don't do it often it is always this that bothers me the most, so here is how
to install OSSEC 3.6.0 as a local agent on a Debian 10 OS, has worked the same
for x64 and ARM systems so far.

First get those packages in line, I usually run a minimalist version of Debian
so these may not come with your current installation.

*apt install wget build-essential libpcre2-dev zlib1g-dev inotify-tools libevent-dev libssl-dev*

With that out of the way pull the package for OSSEC 3.6.0:

*wget https://github.com/ossec/ossec-hids/archive/3.6.0.tar.gz*

Unpack the newly downloaded .tar.gz

*tar -zxvf 3.6.0.tar.gz*

Go into the folder, search for *install.sh* and run it then do local, again
this should also work for other modes of OSSEC but I usually do local and pull
logs through other methods.

Have fun!

