---
title: "Thoughts on Alienvault USM Appliance"
date: 2021-09-28T22:20:32-07:00
draft: false
description: Personal thoughts on Alienvault USM Appliance
tags: ['alienvault','att cybersecurity','USM Appliance','SIEM']
---

It has been some time since I began working in the realm of Information Security,
at this point it is more than five years now, with a total of ten years and a
bit more inside IT.

One of the things that caught my eye as soon as I joined the Information Security
ever growing field was the need for efficient tools that give you visibility.

The SIEM is probably one of the most complicated and interesting platforms I
have ever found in my time in IT, it does a number of things that simplify your
life when trying to find the source of a malicious actor or clarification on a
false positive (which is most of the time).

The problem that quickly raises is how to properly tune this, and in turn how to
make it so that you can remove all of the noise from the wire.

Easier said than done, not to mention the overloading of the platform and the
need to add more power to your platform so that correlation works.

Sounds confusing? It is "easier" once you take a look the first time and
figure out what is going on.

The evolving problem with this type of platforms is optimization and finding
a way to make it do more without drowning the device, this was the initial
case I wanted to study with Alienvault, now AT&T Cybersecurity, USM Appliance.

The platform is not very solid, it has a rather awkward user interface with
some rather interesting decisions, such as why reports have an awkward method
of creation, how modules for reports have to be created and why the platform
uses three types of databases for event normalization.

Creating plugins is another interesting step in the platform, as well as adding
rules and directives.

In general this platform may have been great when it came out into the market
and up to 2016... but in 2021 this platform is dated and may very well be
sun-setting for the newer offering from AT&T Cybersecurity (USM Anywhere).

The scalability of USM Appliance is also... interesting, while there is a
theoretical number of agents, events being processed and event stored that
the company promotes this is not necessarily the case.

But not to say this is bad, reality is always more complicated and in this
case while it may not be the case it is still manageable for businesses that
are small or medium, for larger companies or volumes of data this becomes
considerably complicated.

Some functions that are lacking here are the ease of integration with other
types of databases or turning the data into a lake for tapping through other
means, consider Elastic when thinking about large volumes of data.

There is an open source version available, OSSIM, but the usage is much
smaller as only two of the three components are available.

With USM Appliance you have a Sensor, a Server and a Logger, the Sensor
receives and normalizes logs, the Server correlates everything and the Logger
stores data after a period of time.

With OSSIM there is no offering for long term storage, although it could be
possible to integrate something manually to store the data for the long term this
would need to be coded on the side.

So is USM Appliance viable in 2021? Possibly, the OTX initiative is very good and
provides a great way to keep up to date with all the global threats affecting
other Alienvault users around the world.

Consider other options inside your realm of possibilities before jumping to
USM Appliance "because of the price"... because it may be cheaper but those
savings are exchanged for the man hours you need to troubleshoot issues and
contact support to get assistance.

During my tenure using the platform in a SOC for different clients I noticed
that any troubleshooting could take a considerable amount of time, the problem
lays in the foundation of the platform.

USM Appliance and OSSIM are the same, constructed from open source tools which
have been put together to process the data it is no surprise that components
can easily break down without a clear way to handle it.

Support may need to get your issue sent over to Spain, either to be handled
by a developer or the highest level of support available in the company.

In the end this means odd hours for support and potentially a very long response
time.

Another problem which considerably complicated situations was the inconsistency
of information across support, as if information was not centralized or easily
available already even their own internal departments had this problem.

So again, would I recommend USM Appliance for any business?

Yes and no, for smaller businesses this may be a viable solution without a heavy
investment but I would still recommend OSSIM first before diving into the paid
version.

