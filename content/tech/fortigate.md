---
title: "The History of a Firewall: How to buy a used Fortigate and not die in the process"
author: "Vicente Manuel Muñoz Milchorena"
date: 2018-03-07
tags: ["fortigate", "fortinet", "60C", "troubleshooting", "journey"]
categories: ["fortinet", "journey"]
description: "My misadventures with a Fortigate-60C."
---

# The History of a Firewall: How to buy a used Fortigate and not die in the process

## Introduction: When Overkill Meets Reality

While talking with a friend, he asked for my input regarding a firewall option for his small business. He needs something to replace a server which he could be using for whatever else he could think of - we're talking about a rack-type server with something akin to two Xeons and 16GB of RAM for an office with less than 20 workers who are only there during some times of the day.

**Overkill** is the right word here.

Asking around, many solutions came up: Cisco ASA, FirePower, Palo Alto, SonicWall, and Fortinet were some of the names that rang the most. I was somewhat familiar with Fortinet and the price range was reasonable for what it does in general, but I decided to take a dive onto eBay and managed to squeeze some money out of a **FortiGate 60C** - it ended up being $50 with tax and shipping.

This was a slippery slope to *"what the fuck is going on and what am I doing?"*

## ⚠️ **CRITICAL WARNING for Used FortiGate Buyers** ⚠️

**Before you buy ANY used Fortinet equipment, understand this:**

- **Registration Lock-in**: Fortinet ties firmware downloads to device registration
- **Previous Owner Issues**: Devices registered to previous owners cannot be re-registered
- **No Unauthorized Reseller Support**: eBay sellers are not authorized resellers
- **Firmware Access**: Without registration, you cannot download official firmware
- **Support Limitations**: No paid support options for unregistered devices
- **Terms of Service**: Fortinet's ToS explicitly prohibits unauthorized resales

**Bottom line**: You're buying hardware that may be completely unusable without access to unofficial firmware sources. Proceed at your own risk.

## Initial Hardware Assessment

### Delivery and First Impressions
Getting the device was fast and easy. Opening the box confirmed that I had received what the seller specified - a genuine FortiGate 60C in decent physical condition.

### Power-On Issues
Plugging it in gave me nothing but:
- **Power light**: Solid (good sign)
- **Status light**: Blinking, then going off (not so good)

Looking at the documentation, it wasn't clear what was going on, but getting no IP address from the device on the LAN ports made it clear something wasn't right.

### Initial Troubleshooting Attempts
I tried following the standard procedures:
- **Web UI Access**: Could never reach it, even with static IP addresses
- **No Routing**: Device completely unresponsive to network traffic
- **WAN Port**: Would not obtain an IP address through DHCP
- **Verdict**: The appliance appeared completely dead

This is when I complained to the seller, who provided no feedback about not specifying that the appliance had either been wiped clean or that configurations were still in place. The only way to figure this out was to get an RJ45 to DB9 cable and work my way through the console.

## Console Access Setup

### Hardware Requirements
If you're wondering how console access even works in 2018, let me tell you that I keep an old laptop for this exact purpose:
- **Console Machine**: Toshiba PIII laptop with Windows 98
- **Why**: I love doing retrocomputing (guilty as charged), and I don't have a docking station for my T400, otherwise I wouldn't have to use this ancient beast
- **Cable**: RJ45 to DB9 serial cable
- **Software**: PuTTY for terminal emulation

### Console Connection Settings
```
Baud Rate: 9600
Data Bits: 8
Parity: None
Stop Bits: 1
Flow Control: None
```

## The Firmware Nightmare Begins

### Diagnostic Mode Discovery
Bringing the connection up with PuTTY showed me a screen asking if I wanted to do a test with or without express card. I was baffled by this, but I got it to go through the test without an express card. It indicated failure with the USB and Ethernet ports (because I was missing the loopback wiring it requires, but I wasn't looking to make any tests - I wanted to get this working).

**The revelation**: I was running a **troubleshooting firmware** that came with the FortiGate, which was in the backup section. There was **no main firmware** loaded onto the device, so this took me to the next logical step - getting an account with Fortinet and registering the device.

This is where the *"what the fuck is going on"* part gets **really** interesting.

## Fortinet Support Hell

### Registration Attempt
I tried to register the device with the serial number, but the system would only ask me to contact Support. When I contacted Fortinet Support, they delivered the bad news:

### Support Response Breakdown
1. **Previous Registration**: This appliance was registered under a different name, and they could not provide additional information on this
2. **Unauthorized Reseller**: The seller I bought it from was not an authorized reseller of Fortinet
3. **No Paid Support Options**: There was no support plan I could pay for to get assistance with this
4. **Refund Recommendation**: I was told to get a refund from the seller because this was pointless
5. **Terms of Agreement**: I was told to read the ToS (should do that next time I try to do something this stupid - DUH! Bad customer, you used to work customer support, how could you not know this!)

### The Firmware Lock-out
If at this point you're wondering why this is important, well friend-o, the whole reason this is breaking my neck is because **without this type of support I cannot get the firmware for the device**. Even though I have it physically with me and I paid money for it, I could not get what I needed.

I also read about doing the one-month free FortiGuard trial or some such, but I could not find this option. My Google-fu was weakening because my brain was drying out at this point.

## The Unofficial Solution

### Community Firmware Sources
I did not despair though. I assumed a good Samaritan somewhere in the world would be kind enough to have the firmware somewhere in the public, even if that meant getting shit-canned by Fortinet.

My assumption was right - I found it in an open FTP folder on a website (which I will not name to avoid issues, but if you need directions, contact me privately).

**Disclaimer**: Using unofficial firmware sources carries risks:
- **Legal implications**: May violate Fortinet's terms of service
- **Security concerns**: Firmware authenticity cannot be guaranteed
- **Support voiding**: Definitely voids any potential future support
- **Your responsibility**: I'm documenting my experience, not recommending this approach

### Firmware Version
Finally, I had the **FortiGate 60C 5.2.8 firmware**. Fuck the cookbook - I had this and only this. Following the damn recommended upgrade path was out of the picture at this point.

## The TFTP Upload Process

This takes us to the *"what am I doing?"* section.

### The Technical Challenge
Picture this scenario:
- **Console Machine**: Windows 98 computer (only machine that can connect to FortiGate console)
- **Primary Machine**: Debian Linux (where I downloaded the firmware)
- **Network Setup**: Need TFTP server to push firmware to FortiGate
- **Constraint**: Don't want to set up TFTP on Debian for a one-time operation

### The Rube Goldberg Solution

#### Step 1: TFTP Server Setup
- Downloaded **tftpd32 version 3.0** (works fine on Windows 98)
- Problem: Can't get firmware onto Windows 98 directly (browsers won't work on the download site)

#### Step 2: File Transfer Chain
```
Internet → Debian Machine → Samba Share → Windows 98 → tftpd32 folder
```

**Security Note**: Yes, I set up Samba on my Debian machine accessible to Windows 98. How vulnerable am I? Very. Don't do this on a production network.

#### Step 3: Network Configuration
- Set up DHCP server in tftpd32
- Connected Windows 98 machine to **Port 1** on the FortiGate (after reading through console messages)
- Configured appropriate IP ranges for TFTP transfer

#### Step 4: Console Commands
From the FortiGate console (in recovery mode):
```
# Set up network for TFTP
execute restore config tftp <firmware_filename> <tftp_server_ip>
```

### Upload Success
Was I successful? **Of course!** 
- **Transfer Time**: About 1-2 minutes to move the image
- **Installation Time**: Another 1-2 minutes for the appliance to be ready
- **Result**: As soon as it was done, the FortiGate came to life

The device was finally alive, and I could access the Web UI as promised (no, I just did not want to deal with the console at this point, thank you very much).

## Initial Configuration and Testing

### Web Interface Access
With the firmware installed, the FortiGate was accessible via:
- **Default IP**: 192.168.1.99 (typical for FortiGate 60C)
- **Default Credentials**: admin/no password
- **Web Interface**: Fully functional FortiOS management

### Registration Attempts (Continued Failure)
After changing some basic configurations, I confirmed that I still could not register the appliance no matter what I tried. However, the device was fully functional for basic firewall operations.

### Internet Connectivity Test
I got the device connected to the internet and began experimenting with configurations, which were easier than I thought. The FortiOS interface is actually quite intuitive once you get past the initial hurdles.

## Advanced Configuration: Splunk Integration

Was I done? Oh no, I also had to try one last step before I was happy with what I had done.

### Splunk Light Setup
I had to forward the logs to **Splunk Light** (because I have no hardware that can run the Enterprise version). 

#### Configuration Steps:
1. **Splunk Configuration**:
   - Opened UDP port 514 to receive syslog messages
   - Configured data inputs for FortiGate log format

2. **FortiGate Syslog Configuration**:
   ```
   config log syslogd setting
       set status enable
       set server <splunk_server_ip>
       set port 514
       set facility local7
   end
   ```

3. **Log Flow Verification**:
   - Could see logs slowly moving in (I'm just one person testing it out)
   - Downloaded the FortiGate App for Splunk
   - While functional, I see little purpose for it in my home lab environment

## Lessons Learned and Final Thoughts

### What Worked
- **Console Access**: Essential for any serious FortiGate troubleshooting
- **TFTP Recovery**: Reliable method for firmware installation when official channels fail
- **Basic Functionality**: Device works perfectly for firewall operations without registration
- **Configuration**: FortiOS is intuitive once you gain access

### What Didn't Work
- **Official Support**: Completely locked out due to previous registration
- **Firmware Updates**: Cannot access newer firmware through official channels
- **Registration**: Impossible to re-register used enterprise equipment

### The Real Cost Analysis
- **Purchase Price**: $50
- **Time Investment**: ~8 hours of troubleshooting
- **Risk Factor**: High (potential paperweight without unofficial firmware)
- **Learning Value**: Extremely high
- **Practical Value**: Mixed (works great, but limited upgrade path)

## Quick Reference Guide for Future FortiGate Buyers

### Pre-Purchase Checklist
- [ ] **Verify firmware is installed** (ask seller to confirm web interface accessibility)
- [ ] **Check serial number registration status** (if possible)
- [ ] **Understand firmware limitations** before purchase
- [ ] **Have console access capability** (DB9 serial port + cable)
- [ ] **Research model-specific firmware availability** in unofficial sources

### Essential Console Commands

**Basic Navigation:**
```bash
# Get system information
get system status

# Show configuration
show

# Enter configuration mode
config system admin
    edit admin
        set password <new_password>
    next
end

# Network interface configuration
config system interface
    edit <interface_name>
        set ip <ip_address> <netmask>
        set allowaccess https ssh ping
    next
end

# Save configuration
execute backup config tftp <filename> <tftp_server>
```

**Recovery Mode Commands:**
```bash
# Boot from TFTP (in recovery mode)
execute restore config tftp <firmware_file> <tftp_server_ip>

# Format boot device (EXTREME CAUTION)
execute formatlogdisk

# Factory reset
execute factoryreset
```

### TFTP Server Setup (Windows)

**Using tftpd32:**
1. Download tftpd32 (version 3.0+ recommended)
2. Place firmware file in tftpd32 directory
3. Configure DHCP scope if needed
4. Start TFTP service
5. Connect to FortiGate management port
6. Execute restore command from console

### Common Issues and Solutions

**Problem**: Console shows diagnostic/test mode
**Solution**: Device has no main firmware - requires TFTP recovery

**Problem**: Web interface inaccessible after power-on
**Solution**: Check console for boot errors, likely firmware corruption

**Problem**: Cannot register device
**Solution**: Accept limitation or seek refund - no technical workaround exists

**Problem**: TFTP transfer fails
**Solution**: Verify network connectivity, TFTP server configuration, and file permissions

### Alternative Approaches

If you encounter registration issues:
1. **Request refund** from seller (Fortinet's official recommendation)
2. **Use device offline** for lab/learning purposes
3. **Seek community firmware** (understand risks)
4. **Consider buying from authorized resellers** for future purchases

### Final Recommendations

**For Learning/Lab Use**: Used FortiGates can provide excellent hands-on experience with enterprise firewall concepts, even without official support.

**For Production Use**: **Strongly recommend** purchasing through authorized channels to ensure firmware access and support availability.

**For Budget-Conscious Buyers**: Understand that the "cheap" purchase price may require significant time investment and technical skills to make functional.

---

Hope this helps someone in the future (maybe even myself) and saves some precious hours of their life. Feel free to leave a comment or message me for any questions regarding this adventure in enterprise hardware acquisition!
