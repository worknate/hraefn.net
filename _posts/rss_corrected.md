date: 2006-03-30 17:05:27
title: RSS Corrected
tags:
 - ops
 - code
---

Had some problems getting my RSS feed to work with Gmail's WebClips, so I took a look at it today.  Apparently RSS readers sometimes do not like non-xml file extensions.  Simple enough, now the RSS feed is on a cron job.  Added some things and cleaned up the formatting some while I was in there.

Update: I have since moved to [TextPattern](http://textpattern.com)'s built-in RSS and Atom feeds.

Update 2018-MAR-25: Now [Hexo](https://hexo.io/) generates my [Atom](/atom.xml) feed.