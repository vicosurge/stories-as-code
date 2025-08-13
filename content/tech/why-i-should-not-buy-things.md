---
title: "The History of why I shouldn't buy things I don't know too much about or how I learned to configure a Sun Server SunFire v100"
date: 2018-09-09T22:24:32-07:00
draft: false
description: A very bad idea becomes a journey for tech archeology
tags: ['sunfire','sun microsystems','v100','journey']
---

# The History of why I shouldn't buy things I don't know too much about
## Or how I learned to configure a Sun Server SunFire v100

### Why Old Hardware? A Personal Philosophy

Friends and co-workers have brought this up many times in the past: why would I ever buy used and obsolete hardware when you can find reasonably priced modern hardware on the market? While this is a fair question, and I have nothing against modern hardware, I have a predilection for old systems that can be put back to effective use for two very specific reasons - one is Linux, and the other is a man by the name of K. Mandla.

Back when I started getting into Linux some 10 years ago, I was struggling yet always fascinated by the level of control you had over your operating system. Want to install software? It's probably in a repository. Don't need a GUI? That's fine, run everything through the command line. And it was on the command line where I found myself most intrigued - this almighty black screen held so much control over everything, an esoteric world in front of me that I could not wait to tamper with. Every mistake was a lesson well learned, every success was a thrill, but learning that you could run video and audio directly from the console was something that completely changed the way I saw computers, coming from a purely Windows environment.

When looking for more things to do, I discovered a blog by someone named K. Mandla, and there my love for the console grew. More importantly, my appreciation for old hardware and the possibilities of what I could do with it expanded dramatically. I became fascinated with repurposing vintage systems for very specific tasks. I got into it so much that I went to flea markets and PC stores to pick up old hardware - in most cases I could get it for free because the owners just wanted to get rid of what they considered trash, unable to push it into the modern market.

I was rebuilding old PCs and selling them for $10 or $20 back then. Whatever happened to those computers, I wouldn't know, but at least I managed to rescue them from the garbage dump and gave many people the opportunity of finally having a computer. This was back when having a smartphone meant either an iPhone or a BlackBerry - both expensive options - so a cheap computer, no matter how slow or short-lived, was a viable alternative.

While I'm conscious that using old hardware may mean higher electrical bills (and this is in no way helping the planet or my wallet), there's a very important issue with e-waste being dumped in Africa and Asia. [This documentary provides an informative overview of what's happening in Ghana](https://www.youtube.com/watch?v=mleQVO1Vd1I), where e-waste is literally killing the country.

There's another point aside from repurposing or saving the planet (though this is debatable): collecting old hardware that I had worked with, that was very reliable and apparently very long-lasting. Examples include the IBM T60, T61, and T400 laptops I have at home, which I continue to use even today with Debian as they're more than sufficient for what I need to do. The T60 and T61 are stored now, but I used them for 8 and 7 years respectively. The only reason I stopped using them was RAM limitations, yet I could still see tasks these laptops could handle. Then there's the Pentium III laptop that I use for writing and playing old video games - because while emulation is fine, sometimes it just doesn't work properly, and writing without any interruptions can be golden when trying to put every ounce of effort into it.

There's also the appeal of trying out old hardware I'd never worked with in the past, such as SCSI drives. This was actually more of an accident - I didn't read the specs on the PowerEdge 1850 to see what type of hard drives it used, but at $30 with shipping included, I couldn't pass it up. The hard drives (two 136GB and one 73GB) ended up costing another $20 with shipping. Same story with DAT tapes, which I'd worked with professionally but never had the chance to experiment with personally. While backing up to another hard drive or the cloud is fine, I could always have a spare backup for emergencies on tape.

To add to this inventory, I recently purchased a SunFire v100 and SunFire v120. One uses IDE drives and the other SCSI, so I shouldn't have compatibility issues getting them to work. They already have RAM and everything installed - it's just a matter of connecting through a DB9 to RJ45 cable to the console to get them to boot from CD (which I still have plenty of, surprisingly) and install an operating system so I can experiment with them. Probably set up a small web server or even a firewall/proxy - we'll see. But this was all because I wanted to play around with these old machines, having read about them and being intrigued by their reputation.

The SunFire v120 turned out to be another story entirely (one that requires a whole different document to explain that particular ordeal), but the v100 became the subject of this technical journey.

Now the only problem I have left is buying a server rack and building up a proper room to house all the servers and retro hardware that's just piling up.

### Introduction to the Technical Journey

This document contains detailed information regarding the initial opening, setup, and deployment of the Sun Server SunFire v100. What started as a simple eBay purchase based on curiosity and nostalgia turned into a multi-day odyssey of troubleshooting, learning OpenBoot commands, and wrestling with legacy hardware that taught me more about Sun systems than I bargained for. The following sections detail every step taken and every lesson learned along the way.

### Hardware Overview

The server arrived well-packaged in bubble wrap, packing peanuts, air bags and was inside a cardboard box. The item matched the eBay seller's description and photos perfectly - a genuine SunFire v100 Server with the following specifications:

**Physical Condition:**
- No rails included, but mounting ears present
- One mounting ear showed some surface oxidation
- Internal condition was excellent and clean, suggesting recent decommissioning
- Sun configuration card present on the back panel

**Hardware Specifications:**
- **Storage**: Two IDE hard drives, 40GB each
- **Memory**: 2GB PC133 RAM configured as 4 × 512MB modules
- **Processor**: Mounted with heatsink (left undisturbed during initial inspection)
- **Connectivity**: Serial console access via A/LOM port

**Purchase Details:**
- **Total Cost**: $35.00 USD (July 2018)
- **Item Cost**: $12.50 USD
- **Shipping**: $22.50 USD
- **Received**: July 14th, 2018 (Saturday)

### Initial Setup and Discovery

#### Power-On Procedure
Unlike modern servers that power on when connected to mains power, the SunFire v100 requires manual activation. The power switch wasn't initially engaged, which led to some initial confusion.

#### Console Connection Setup
To access the system console, I used:
- **Client Machine**: Toshiba Satellite running Windows 98 with native serial port
- **Cable**: DB9 to RJ45 adapter
- **Connection Point**: A/LOM (Alternate Lights Out Management) port on server rear panel

#### First Boot Experience
The LOM interface revealed the server was in standby mode. Using LOM commands, I successfully powered on the server, which initiated a full boot sequence into **Solaris 5.10**. Interestingly, the system was pre-configured with an AT&T business IP address, though no sensitive data remained on either hard drive.

### Major Issues Encountered

#### Boot Failure and Network Boot Mode
After the initial successful boot, subsequent restart attempts failed. The server had inexplicably fallen back into network boot mode, attempting to PXE boot from a non-existent server. This behavior was apparently common with Sun systems of that era, designed for centralized network-based deployments.

#### The Boot Mode Solution
To break out of the network boot cycle and access OpenBoot:

1. Access LOM prompt
2. Execute: `bootmode forth`
3. Power on the server: `LOM> poweron`
4. System drops to OpenBoot prompt instead of attempting network boot

#### CD/DVD Boot Failures
Initial attempts to boot from optical media resulted in **Fast Data Access MMU miss** errors, suggesting hardware issues within the server. This error can indicate various internal hardware problems and prevented any installation media from loading properly.

### Troubleshooting Deep Dive

#### OpenBoot Configuration
Following guidance from [Oracle's IT Toolbox documentation](https://it.toolbox.com/question/fast-data-access-mmu-miss-in-sunfire-v120-061010), I implemented the following OpenBoot configuration changes:

```
setenv diag-level? max
setenv auto-boot? false  
reset-all
```

These commands:
- **diag-level? max**: Enables maximum diagnostic verbosity during boot
- **auto-boot? false**: Prevents automatic boot attempts, keeping system at OpenBoot prompt
- **reset-all**: Applies changes and performs system reset

#### The RAM Problem
Despite successfully reaching OpenBoot, boot warnings indicated potential issues with **DIMM0**. Initially, I removed this module and continued, but the problems persisted. This should have been my first clue to investigate the memory subsystem more thoroughly.

#### Installation Attempts and Failures
Multiple operating system installations failed:
- **Debian**: Installation ran for approximately 24 hours before crashing
- **OpenBSD**: Similar failure pattern
- **NetBSD**: Also failed to complete installation

Following advice from the **Backyard Tech** YouTube channel (specifically content about SunFire v120 troubleshooting), I burned installation media at the lowest possible speed to minimize read errors. Results remained inconsistent.

#### Controller vs. Media Investigation
To isolate the problem, I swapped the CD-ROM drive with one from my SunFire v120 (another story entirely). The same media failures occurred, pointing to an issue with the internal controller rather than the optical drive itself.

### The Solution: Memory Management

#### Complete Memory Removal and Testing
The breakthrough came when I removed **all** RAM modules and began systematic testing. The root cause was indeed faulty memory - either defective modules or malfunctioning DIMM slots.

#### Final Memory Configuration
**Critical Note**: RAM installation in SunFire v100 follows back-to-front ordering (clearly documented on the system's cover plate).

**Working Configuration**: Single 512MB module installed in **DIMM3** slot.

With this minimal memory configuration, all previous issues resolved immediately.

### Successful System Configuration

#### OpenBoot Documentation
For comprehensive OpenBoot configuration, Oracle maintains excellent documentation at: [OpenBoot Configuration Guide](https://docs.oracle.com/cd/E23824_01/html/821-2731/gkkvd.html)

This resource provides:
- Complete OpenBoot command reference
- Drive probing procedures (previously failing SCSI/IDE detection now worked)
- Boot device configuration for automatic startup

#### Current Boot Behavior
With proper OpenBoot configuration, the system now boots directly to Debian without requiring LOM intervention. The Windows 98 machine now serves purely for resource monitoring via LOM interface.

### Operating System: Debian SPARC64

#### Installation Success
Debian installation completed successfully once the memory issues were resolved. However, the SPARC64 architecture presents unique challenges in the modern package ecosystem.

#### Repository Challenges
The Debian SPARC64 port has limited package availability:
- **Issue**: Main repository failed to connect during installation
- **Workaround**: Installation proceeded with packages from netinst CD only
- **Reference**: [Debian SPARC64 Wiki](https://wiki.debian.org/Sparc64) contains platform-specific configuration instructions

#### Package Availability Issues
**Available Packages:**
- apache2 web server
- build-essential (compilers and build tools)
- python3 and pip3
- PostgreSQL database

**Missing/Problematic Packages:**
- MariaDB Server 10.1
- MongoDB
- Most MariaDB build dependencies
- Security repository for 'sid' branch (non-functional)

#### Current Database Compilation Project
For the past two days (Thursday 19th and Friday 20th), I've been manually compiling libraries and dependencies to get MariaDB running on the SPARC64 platform. While SQLite or PostgreSQL would be simpler alternatives, I wanted MySQL/MariaDB compatibility in my Debian build.

### Lessons Learned

#### Hardware Diagnosis Hierarchy
1. **Memory issues often manifest as seemingly unrelated boot problems**
2. **Systematic component removal is more effective than assumption-based troubleshooting**
3. **Legacy hardware documentation is invaluable - Oracle's OpenBoot guides are comprehensive**
4. **Architecture-specific software limitations require research before purchase**

#### Sun Hardware Specifics
- **LOM provides powerful remote management capabilities**
- **OpenBoot is incredibly flexible once properly understood**
- **Network boot was designed for enterprise environments, not home labs**
- **Memory slot ordering matters and is system-specific**

### Quick Reference for Future SunFire v100 Users

#### Essential Commands

**LOM (Lights Out Management):**
```
LOM> poweron           # Power on the server
LOM> poweroff          # Power off the server  
LOM> bootmode forth    # Boot to OpenBoot instead of OS
LOM> console           # Access system console
```

**OpenBoot:**
```
boot cdrom                    # Boot from CD/DVD
boot disk                     # Boot from primary disk
setenv auto-boot? false       # Disable automatic boot
setenv diag-level? max        # Maximum diagnostic output
probe-ide                     # Detect IDE devices
probe-scsi                    # Detect SCSI devices
show-devs                     # List all devices
printenv                      # Show all environment variables
reset-all                     # Apply changes and reset
```

#### Hardware Configuration Notes

**Memory Installation:**
- Install RAM from back to front (DIMM3 → DIMM2 → DIMM1 → DIMM0)
- Test with single module first if experiencing boot issues
- PC133 SDRAM required

**Storage:**
- Supports IDE and SCSI drives
- Primary boot device configurable via OpenBoot
- CD-ROM/DVD drives may require specific models for reliability

**Console Access:**
- Requires DB9 to RJ45 cable for LOM connection
- Serial settings: 9600 baud, 8N1
- Modern USB-to-serial adapters may work but not guaranteed

#### Troubleshooting Quick Fixes

**"Fast Data Access MMU miss" Error:**
1. Check memory configuration first
2. Try single RAM module in DIMM3
3. Verify all modules are properly seated
4. Test with different RAM if available

**Boot Loops/Network Boot Issues:**
1. Access LOM: `bootmode forth`
2. Power on to reach OpenBoot
3. Configure boot device: `setenv boot-device disk`
4. Disable auto-boot if needed: `setenv auto-boot? false`

**Operating System Considerations:**
- Debian SPARC64 has limited package availability
- Consider Gentoo or *BSD for better SPARC support
- Compile times will be significant on single-core systems
- Plan for manual dependency compilation for modern software

#### Final Thoughts

The SunFire v100, while an interesting piece of computing history, requires patience and a willingness to work within the constraints of both legacy hardware and niche architecture software support. The experience provided valuable insights into enterprise Unix systems and the importance of thorough hardware diagnosis.

For anyone considering similar vintage Sun hardware: budget time for troubleshooting, maintain good documentation, and remember that sometimes the simplest explanation (like bad RAM) is the correct one, even when the symptoms seem complex.
