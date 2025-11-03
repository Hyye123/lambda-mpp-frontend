# Guide for bot developers and coders on Lambda MPP

[Русский (Russian)](https://docs.google.com/document/d/1aKQhIQPHTxVGoZy_osGbKXPBZ71q5Uhd2_Rysl-xkBo/edit?usp=sharing)

This guide contains general information for what you’re allowed to do with bots, may be risky to do with bots, and some tools to help you get started.

[Original MPP Bot Guide](https://docs.google.com/document/d/1OrxwdLD1l1TE8iau6ToETVmnLuLXyGBhA0VfAY1Lf14/edit?usp=sharing) was used as a basis.

# **Good uses for bots**

* Fake currency system with games users can play  
* Tracking name history and letting users view it  
* Bridging chat in some channels to other places (such as Discord)  
* Managing a room or keeping the crown in it while the owner is offline/afk  
* Finding where users are on the site  
* Midi players  
  * Try to avoid using midi players in rooms where the room owner does not permit midis  
* Translation  
* Chatting with an AI  
* Learning to program
* Bots that play the piano together, or make animations together.

# **What’s not allowed**

The bot and the bot’s owner may be banned for doing any of the following:

* If your bot iterates through all channels, for example to track name history or find users, you must skip rooms with "noindex": true in channel settings. This is a channel setting meant to allow users to opt-out of bots for privacy reasons. You may include noindex rooms in global player counts. However, any bot automatically joining noindex rooms may be banned.  
* Abusing code or exploits to lag the server or break it in any way (report bugs if you find any\!)  
* Using bots to break any of the [site’s rules](https://github.com/Hyye123/lambda-mpp-frontend/blob/main/rules.md)  
* Using bots with the intention of making the site worse for others  
  * Example: A bot that goes around rooms automatically and insults users  
  * Example: A bot that goes around rooms automatically and spams the piano until it gets banned  
* Using the site normally through a user that has a BOT tag  
  * The tag is meant to represent that a user is not a real human. For this reason, the BOT tag will not be given out to bots which run in a browser.  
* Spamming the server with messages that don’t do anything  
  * Example: Attempting to set the bot’s name every second when its name is already the desired name  
  * Example: Sending “ch” to join a channel on an interval when the bot is already in that channel  
  * Example: Sending hundreds of cursor movements per second (the server limits cursor movements to 20 per second)

## Gray area:

There are some things a bot can do which are not explicitly allowed or disallowed, and will be dealt with on a case-by-case basis

* Using bots to take the crown in a room  
  * If a room owner has left, using a script that automatically picks up the crown is allowed as long as you return it to the owner if they rejoin.  
  * Using a bot to wait until someone else's room is abandoned, then holding the crown there forever, is not allowed.  
* Attempting to bypass the anti-bot system  
  * Analyzing and trying to reverse-engineer the code is allowed  
  * If you test out a bypass and get auto-banned, you will not be unbanned

# **Important note about security**

Do not trust the server when developing bots. It's possible for the server to send your bot malicious/fake messages through an exploit or compromise on our end. If you decide to implement an eval command or something similar, keep it sandboxed or the liability is on you.

# **The anti-bot system**

Bots in Multiplayer Piano have historically been abused to crash the site, flood chat, lag players by spamming the piano, and exploit bugs. As a result, an anti-bot system has been implemented, and bots are much more restricted now. Getting detected by the anti-bot system will result in a 24 hour ban. That ban is not meant much as a punishment, but more as a way to make it more difficult to reverse-engineer. If you accidentally get yourself banned, message me on Discord (hyye.xyz) and there's a good chance I'll remove it. Evading the auto-ban is not allowed if you intentionally tried to bypass it.

# **Bots in a browser**

If you would like to create a bot in the browser, there are some guides for [beginners](https://gist.github.com/Hri7566/34565985e54632f815f15323704a0919) and [intermediate](https://gist.github.com/Hri7566/117511ec6888db2cf4c1debde1d1a9b6) JavaScript programmers. One common way to use these is with [Tampermonkey](https://www.tampermonkey.net/).

Bots in the browser do not require approval. Due to the nature of the anti-bot system, there are limits on what you can do with them.  
In general, adding on to the site should be safe, for example, adding event listeners to MPP.client and calling functions in the global MPP object are allowed and mostly safe to do. Changing the theme of the site (CSS and HTML) are usually fine as well.  
Modifying existing properties or functions can lead to a ban, especially in Client.js.

# **Bots outside of a browser**

All bots that don’t run in a browser require approval. This is because they are significantly easier to abuse. Your client will need to send a bot token every time it connects to authorize itself. See [the protocol documentation on this](https://github.com/mppnet/frontend/blob/main/docs/protocol.md). To get a token, message me on Discord (hyye.xyz).

Bots that run outside of a browser and use a token will get a BOT tag. This tag represents that a user won’t be exploring rooms, chatting, and playing the piano as a human and that the user is only a bot.

## **Node.js**

If your bot runs in Node.js, your best choice is [mpp-client-net](https://www.npmjs.com/package/mpp-client-net). This module supports tokens by default and is made to be used with MPPNet (compatible with Lambda MPP).

If you are a bit more experienced, it is recommended that you use the [dotenv](https://www.npmjs.com/package/dotenv) module to keep your token secret, but we can't provide exact instructions for any given scenario.

## **Deno**

If your bot runs in Deno, your best choice is [mppclient@v1.0.0](https://deno.land/x/mppclient@v1.0.0) (also known as mpp-client-deno). This module is functionally identical to [mpp-client-net](https://www.npmjs.com/package/mpp-client-net) and supports token-based authentication out of the box.

## **Other languages**

If you know a client library for another language or write one, contact hyye.xyz on Discord and it may be added to this document.

# Source code and protocol documentation

The server’s source code is private. The client’s source code is public and can be found [here](https://github.com/Hyye123/lambda-mpp-frontend). Feel free to open issues or make pull requests for new features or bug fixes. For information on the protocol lmpp.hyye.xyz uses, see the [protocol wiki page](https://github.com/mppnet/frontend/blob/main/docs/protocol.md).