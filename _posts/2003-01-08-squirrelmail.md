---
date: 2003-01-08 06:00:00
title: SquirrelMail and SSL
tags:
 - ops
 - code
---

We decided to try out web-based mail in an attempt to finally get away from Lotus Notes.  We wanted something free and relatively open-source, specifically running off of Perl or PHP.

I picked SquirrelMail because I have had previous good experience with it, and Horde/IMP required a whole lot of re-compiling of PHP.

In order to connect SquirrelMail to an IMAP/SSL server, I compiled stunnel
http://www.stunnel.org and ran the following command as recommended by the
SquirrelMail team:

    /usr/sbin/stunnel -P/tmp/ -c -d 10143 -r mail.yoursite.com:993

Then placed this command in `/etc/rc.local`

This basically maps **localhost:10143** to **mail.yoursite.com:993** (the IMAP/SSL port), so that SquirrelMail can log in. SquirrelMail then just needs to be configured to connect to **mail.yoursite.com:10143.**

Some settings notes for SquirrelMail:

Use the Perl configuration script under `/opt/http/sqmail/config/conf.pl`

    SquirrelMail Configuration : Read: config.php (1.2.0)
    ---------------------------------------------------------
    Server Settings
    1.  Domain               : yoursite.com
    2.  IMAP Server          : localhost
    3.  IMAP Port            : 10143
    4.  Use Sendmail/SMTP    : SMTP
    6.    SMTP Server        : mail.yoursite.com
    7.    SMTP Port          : 25
    8.    Authenticated SMTP : false
    9.    POP Before SMTP    : false
    10. Server               : uw
    11. Invert Time          : false
    12. Delimiter            : detect

To check if stunnel is running right

    ps -elf | grep stunnel

If you don't see the process,

    restart: /usr/sbin/stunnel -P/tmp/ -c -d 10143 -r mail.yoursite.com:993

