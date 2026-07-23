+++
title = "Home server changes and shenanigans: Chapter 0"
description = "because I can't think of a better title lol"
date = 2026-07-22

[extra]
accent_color = "#8F4C36"
accent_color_dark = "#FFB59E"
banner = "banner.png"
card = "banner.png"
+++

Instead of something depressing and negative like the last few posts, I actually have something positive to post about today!

# A little history
About... 2 years ago, I had an idea to turn my gaming computer into a home server. I got some SSDs, mounting brackets, some RAM for the old laptop that I got from my stepdad (that I was originally going to use as a personal computer, more on that later), and a bunch of other things, since I was planning on making a video about this process. That, somewhat thankfully looking back on it, didn't happen. Instead, that old laptop ended up getting turned into my home server. As of now, I currently have it running Debian with Navidrome and Nextcloud on it, down from a lot more services that were running on it the first time when I was using Fedora Server and Docker.

I say all of that because recently, I realized that the battery of the laptop that I got from my college for school has been dying. To be fair, it really shouldn't be a surprise given how I used it like a desktop, keeping it on the charger 24/7 and really only using it at my desk. I configured it to preserve the battery as much as possible, but I guess that couldn't save the battery from dying eventually. Oh well. It lasted about 5 years, so that's pretty good in my eyes. I could possibly get a replacement battery, but I would need to also find a screwdriver that could get into the rather shallow screws on the bottom. Plus, even if I did replace it, the finicky keyboard makes it rather hard for it to act as a laptop like it's supposed to, so I've been using it with a wireless keyboard. So, I thought, "With the laptop's battery dying, what could I possibly use the laptop for?"

...

What about another home server?

# Yet another home server
For a bit, I've been thinking about possibly getting a mini PC or another Raspberry Pi to host some more stuff, like Immich or possibly Ente, or moving Pi-Hole to it to take the load off of the single Raspberry Pi Zero W handling my network's DNS requests. Sure, the laptop home server could probably handle it, but it would involve a lot of manual storage configuration that I didn't want to get into. Why? Because in trying to DIY the server set up this time, I also partitioned the drive myself to some degree, messing around with LVMs and whatnot. I was trying to make sure that I really understood what was running on the system this time, and at least to me, it's actually paid off (would I do it again? Maybe not). I never thought of using the school laptop until now, since I was using it as a personal computer running Fedora Linux. On top of that, I realized that I could transform the main home server into a dedicated NAS running something like OpenMediaVault, getting me away from using Nextcloud (not that I have anything against it; I just don't make use of a lot of things on it). So, I started brainstorming and thinking about what I would need to do to eventually get to this point and eventually came up with a plan:

First, I need to move to another computer as a personal device. I had a behemoth of a desktop tower that my dad gave me, but that's for running Windows; I wanted to run desktop Linux on it. So, I decided to go for that old gaming computer that I was originally planning to turn into a home server. At this point, it was serving as a light gaming computer running Bazzite, something nice and cozy separate from Microsoft. Second, I need to install Debian on the laptop and set it up as a server. Until I could confirm that everything was working correctly, this would involve setting it up with Navidrome, Pi-Hole, and even Caddy to serve as a single reverse proxy for my services. Eventually, I'll add more services to it as well. Finally, I'll install OpenMediaVault on my current main home server to set it up as a NAS. Sure, I don't have 80TB of storage to use, or even the means to manage that much storage, but I did have a good bit of storage that I still haven't completely used up.

As of writing, I've already finished with the first step. I'm writing this on that gaming computer to run it through its paces before I go to the next step. I'll write about that next part soon.

<details>
<summary>Specs??????</summary>

For those wondering, here are some light specs of the devices mentioned here:

## Main home server
- **Device:** Dell Inspiron 15 5555
- **CPU:** AMD E2‑7110
- **RAM:** 16GB DDR3
- **OS:** Debian 13 (Linux 6.12.41)

## The poor Raspberry Pi handling my entire network's DNS requests lol
It's literally a Raspberry Pi Zero W with a 32GB SD card that I got from a college when they were sending out recruiting mail. Came with a case and some other things, too. It also runs FreshRSS.

## Yet another Dell Inspiron to turn into another server
- **Device:** Dell Inspiron 5402
- **CPU:** Intel Core i7-1165G7
- **RAM:** 16GB DDR4
- **Storage:** 512GB
- **OS:** Fedora Linux 44 KDE Edition

## The gaming computer to move to
- **CPU:** Intel Core-i5 10400F
- **RAM:** 16GB DDR4
- **Storage:** 500GB
- **GPU:** GeForce GTX 1650 Super
- **OS:** [Bazzite](https://bazzite.gg/)
</details>