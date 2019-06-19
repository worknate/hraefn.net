---
date: 2004-10-26 17:19:25
title: Obnoxious Lucidity
tags: [ops,code,applescript]
---

A few days ago I started to receive unsolicited AIM messages from a form of chatterbot under the screen name of Eliza6070.  A quick google shows me that the [GAIM](https://en.wikipedia.org/wiki/Pidgin_(software)) client allows for scripting and there are several chatterbot scripts available.  The performance of this particular "bot" was underwhelming... the original Eliza and [various](http://www-ai.ijs.si/eliza/eliza.html) [derivatives](http://www.manifestation.com/neurotoys/eliza.php3) where at least entertaining.  This bleating from my iChat interface..  I've written better and not tried very hard.

All this bot seemed interested in was getting more AIM screen names. I decided to accommodate the poor bastard with ten minutes of my own Applescript.

```applescript
  tell application "iChat"
    activate
  end tell

  set dbl_quote to "\\""

  repeat
    set currentYear to year of (current date)
    if currentYear is 2010 then exit repeat

    -- You gotta love GUI Scripting
    set alphabet to "abcdefghijklmnopqrstuvwxyz"

   tell application "System Events"
      tell process "iChat"

        -- Valid AIM [bot] lengths; 5 to 16
        set wait_time to random number from 2 to 5
        set word_length to random number from 5 to 16
        delay wait_time
        keystroke dbl_quote
        repeat word_length times

          -- Add some random delay
          set chib to random number from 1 to 6
          if chib is equal to 1 then delay 1

          -- Type our random character
          set rand_char to some character of alphabet
          keystroke rand_char

        end repeat
        keystroke dbl_quote
        keystroke return
      end tell
    end tell
  end repeat
```

The only caveat: because iChat is not directly scriptable, you have to leave the targeted message window in the foreground, so forget getting anything done.  I may look into a work-around or try GAIM for Mac OS X... but probably not.
