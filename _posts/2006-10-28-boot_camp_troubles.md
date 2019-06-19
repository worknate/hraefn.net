---
date: 2006-10-28 01:52:41
title: Boot Camp Troubles
tag: ops
---

I decided this week to once again install Windows XP onto my MacBook, and set it up with the couple applications I needed for work.  Hauling a laptop to the office is not onerous, but the MacBook is a hell of a lot lighter than the clunker Dell.  Nevermind that I need Windows to run [Ventrilo](http://www.ventrilo.com/) during [guild](http://us.battle.net/wow/en/guild/elune/Titans%20Exodus/) runs.

Having used [Boot Camp](https://support.apple.com/boot-camp) before, I felt pretty comfortable with repartitioning my drive and fiddling with the Apple drivers.  I spent an afternoon clearing some space; I removed ripped DVDs, a seldom-used backup installation of [WoW](https://worldofwarcraft.com/en-us/), some software that came with the MacBook and a lot of downloads and log files. I grabbed the latest Boot Camp and ran the installer.  One gratuitous legal agreement later, and I am attempting to set up a 20gb FAT partition.

    Your disk cannot be partitioned because some files cannot be moved
    
    Back up the disk and use Disk Utility to format the disk as a single Mac OS Extended (Journaled) volume. Restore your information to the disk and try using Boot Camp Assistant again.

Damn.

I was pretty sure from the start the issue was fragmentation.  The intarweb was not much help, but I was able to find a very nice defrag utility for the Mac, called [iDefrag](https://coriolis-systems.com/iDefrag/).  Running a trial version of their software showed me exactly what I expected... the MacBook had no contiguous space large enough to partition.

So I bit.  The software was easy to buy and I soon had a licensed copy.  I rebooted the MacBook into [Target Disk Mode](https://support.apple.com/en-il/HT201462) and plugged it into the G5.  An overnight Optimize cleaned up the drive nicely, but I was not done yet; I still had to run Apple's disk utility to fix a small error that iDefrag had created.  Not a big deal.

Now if only Windows XP did not make me feel so... exposed.