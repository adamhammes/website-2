---
title: "Managing a cloud Minecraft server from a Discord bot"
date: 2020-09-26T15:37:16-07:00
draft: true
---

![Logos (Minecraft, Amazon Web Services, Discord, Go)](/images/bot-logos-raw.png)

2020 has not been especially \*ahem\* bright for the most part. Very much has gone wrong, and very much continues to go worse. But I found a bit of solace after setting up Discord server for some of my friends and I.

If you're unfamiliar, [Discord](https://discord.com/) is a essentially a big chat room-- like Slack or Microsoft Teams or GroupMe-- but geared towards gamers. Communication is organized around "servers", which have any number of "channels" for specific topics, including voice channels, which you join with any microphone equipped device. These voice channels are great for the pandemic era; I've used them for gaming, hosting virtual movie nights, and just catching up.

So Discord ended up being the perfect place to stay up to date without the very linear expectations of a group chat. And although the majority of the members of our server weren't gamers, we spent a lot of time gaming together. Specifically, playing Minecraft.

Minecraft was the perfect game for us mainly due to how simple it is. Its simple controls were great since many of us don't often game and can't be expected to keep up with anything hardcore; its system requirements are fairly minimal, which is essential since only a few of us were playing on anything other than standard laptops; but mostly, the rules of the game are perfectly relaxed. You're not really expected do anything other than make a nice house to sleep in and... make it nicer. 

Minecraft was the perfect game for us mainly due to how simple it is. Its simple controls were great since many of us don't often game and can't be expected to keep up with anything hardcore; its system requirements are fairly minimal, which is essential since only a few of us were playing on anything other than standard laptops; but mostly, the rules of the game are perfectly relaxed. You're not really expected do anything other than make a nice house to sleep in and... make it nicer. 

It's totally possible to play online in a shared game, of course. As long as you're hosting the game.

Minecraft provides [server software](https://www.minecraft.net/en-us/download/) that can create a networked world, but unless you've got a fairly good grip on ip networking and os fundamentals, this is pretty tough stuff. It was time to put that CS degree to work! It's out of the scope of this post, but setting up a custom Minecraft server involved opening a port on my home router's firewall, and forwarding that port to my PC (named Marilyn ❤️) which ran the server software. Anyone that wanted to play on my server would open a direct connection to my router's public ipv4 address.

This worked great-- for quite a while! Friends from across the country came together to build our own little world. But the setup was not without its drawbacks.

Most of the members of our server live on the east coast, while I-- and more importantly, Marilyn-- live in California. Everyone except for me had a fairly poor connection to the server. Most of the time we didn't notice it-- but it reared its head at intensive moments, like the establishing the initial connection and logging in. But more importantly-- I had to manually start and stop the server when we wanted to play. And I couldn't always be around when the desire to play struck! I was suddenly feeling the pressures of a network admin, albeit one with low responsibilities.

But hold on, I didn't need to suffer this fate, we're living in the future! All I really needed was a computer that I didn't have to manage to run our server, and what is the Cloud but networked computers!

I started looking into cloud providers for a solution, and started with Heroku. Heroku is a developer-friendly wrapper over cloud providers like AWS, abstracting away the virtual machines and refocusing around what the virtual machines do: run applications. Easy configuration for things like runtime environments (jvm, python) or connections to databases, but less heavy on the (virtual) metal of a cloud instance. I had recently deployed a web app to Heroku, and figured that it might be the best place to start. I configured a basic "application" that simply consisted of running a java jar file (our server software).

I kicked off a deploy, watched it build, and... it immediately failed. The Minecraft server couldn't even fully bootstrap itself. Setting up my app was really pain-free, thanks to Heroku's platform. But what it cost me was actual ability. Since I was trying to stay in the free tier of any provider - I was limited to really dinky VMs (or dynos, in Heroku-speak). In this case, the max memory for my dyno was 512 megabytes. This is fine for a fairly low demand web application, which dumps json or html to a broswer, but a video game server maintains many network connections over a long period of time. Lump in java's infamous appetite for RAM, and our server ran out of memory even before it could bind to a port.

So I was left scratching my head. I could hopefully mitigate this by just paying for a dyno with more memory, but the dyno tier above the free one didn't have enough memory. In fact, you had to skip quite a few tiers to get a dyno that had what I estimated as sufficient memory to run a game server. And those dynos were far from free.

I refused to throw in the towel so easily. I'll do any amount of work to avoid paying money, god damnit!

So the friendly Heroku was out. I cracked my knuckles 