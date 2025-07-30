---
title: "Installing OSSEC 3.6.0 on Debian 10 (Buster) How-To"
date: 2021-09-14
tags: ["security", "ossec", "hids", "monitoring", "linux"]
categories: ["Security", "System Administration"]
description: "A comprehensive guide to installing and configuring OSSEC HIDS 3.6.0 for host-based intrusion detection"
author: "Vicente Manuel Muñoz Milchorena"
---

This guide covers the installation of OSSEC 3.6.0 as a local agent on Debian 10 (Buster). The process has been tested and works consistently on both x64 and ARM systems.

Because this installation process always catches me by surprise when I need to do it, and while I don't do it often, it's always the setup that bothers me the most. Here's a straightforward guide to get OSSEC 3.6.0 running on your Debian 10 system.

## Installation

### Install Required Dependencies

First, install the necessary packages. I usually run a minimalist version of Debian, so these packages may not come with your current installation:

```bash
apt install wget build-essential libpcre2-dev zlib1g-dev inotify-tools libevent-dev libssl-dev
```

### Download OSSEC 3.6.0

With the dependencies installed, download the OSSEC 3.6.0 package:

```bash
wget https://github.com/ossec/ossec-hids/archive/3.6.0.tar.gz
```

### Extract the Package

Unpack the newly downloaded .tar.gz file:

```bash
tar -zxvf 3.6.0.tar.gz
```

### Run the Installation

Navigate into the extracted folder, locate the `install.sh` script, and run it:

```bash
cd ossec-hids-3.6.0
./install.sh
```

During the installation process:
1. Select your preferred language
2. Choose "local" for the installation type
3. Follow the remaining prompts with default settings

This process should also work for other OSSEC modes, but I typically use local installation and pull logs through other methods for my setup.

## Conclusion

That's it! You now have OSSEC 3.6.0 running on your Debian 10 system. The installation process is straightforward once you have all the dependencies in place.

Have fun monitoring your system!
