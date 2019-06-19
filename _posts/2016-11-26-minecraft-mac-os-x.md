---
title: Minecraft Server on Mac OS X
date: 2016-11-26 23:39:39
tags: [ops,gaming]
background: /assets/images/wallpapers/minecraft-sunset.jpg

---

10.10 El Capitan

1. Install homebrew
2. Install Cask `brew install caskroom/cask/brew-cask`
3. Install Java using Cask `brew cask install java`
4. Run the regular Minecraft launcher

*This installs the latest version of Java 8, and does a little magic for Java 6 app compatibility.*

1. Set the server machine to a fixed ip address
2. Note what port the Minecraft Server starts as
3. Use **Direct Connect** example: 192.168.0.102:25565

### Skins

Just load them against the Minecraft.net account

### Operators

From the server window just type 'op username' and the ops.json file will get updated automatically.

#### Manually
Edit ops.json in a text editor; you can get the UUID from the server window when the user logs in.

    [
      {
        "uuid": "54d61e19-71cc-477d-8215-8a11c41f5211",
        "name": "someguy",
        "level": 4
      }
    ] 

### Server Properties

#### max-world-size
This is a "radius" and defaults to 29999984. Here are some examples:

* 1000 - 2000x2000 world border
* 4000 - 8000x8000 world border
* 432 - Xbox 360 - 864×864 blocks
* 2560 - Xbox One - 5120x5120 blocks

