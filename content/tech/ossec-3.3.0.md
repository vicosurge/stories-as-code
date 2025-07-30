---
title: "OSSEC 3.3.0 HIDS Setup and Usage"
date: 2020-04-04
draft: false
tags: ["security", "ossec", "hids", "monitoring", "linux"]
categories: ["Security", "System Administration"]
description: "A comprehensive guide to installing and configuring OSSEC HIDS 3.3.0 for host-based intrusion detection"
author: "Vicente Manuel Muñoz Milchorena"
---

OSSEC HIDS is an open-source tool used to monitor and assist in preventing unwanted access to a host. Other configurations can be done depending on what is needed for the specific scenario, and integration with monitoring platforms such as OSSIM, Splunk, or ELK are relatively simple.

This guide covers the installation and basic usage of OSSEC HIDS version 3.3.0.

## Installation

### Download and Extract

Download the source code and uncompress it:

```bash
wget https://github.com/ossec/ossec-hids/archive/3.3.0.tar.gz
tar -zxvf 3.3.0.tar.gz
cd ossec-hids-3.3.0
```

### Install Dependencies

The following packages are required, and these can vary depending on whether it's a Debian or Red Hat based system.

#### For Debian/Ubuntu:

```bash
apt install libpcre2-dev zlib1g-dev build-essential
```

#### For CentOS/Red Hat:

```bash
yum install libpcre2-devel zlib-devel libevdev libevent libevent-devel openssl-devel
```

### Configure Build Environment

Run the following command to enable pcre2 for the compilation process:

```bash
PCRE2_SYSTEM=yes
```

### Run Installation Script

Inside the OSSEC folder, run the install.sh script:

```bash
./install.sh
```

During the installation process:

1. Choose your preferred language for the installer and press enter
2. Press enter again to continue
3. For a standalone installation (as shown in this example), select "local"
4. Leave all other options as they are shown by default
5. If email notifications are required, configure an email account and SMTP settings
6. If email notifications are not required, simply type "N" and continue

Once the compilation is finished, OSSEC will be installed under `/var/ossec`.

## Usage

### Starting, Stopping, and Restarting OSSEC

To control the OSSEC service, navigate to the following path:

```bash
cd /var/ossec/bin
```

Then run the control command with the desired action:

```bash
./ossec-control [start|restart|stop]
```

Replace `[start|restart|stop]` with the appropriate action:
- `start` - Start the OSSEC service
- `restart` - Restart the OSSEC service
- `stop` - Stop the OSSEC service
