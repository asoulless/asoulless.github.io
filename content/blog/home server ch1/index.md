+++
title = "Home server changes and shenanigans: Chapter 1"
description = "something something NVIDIA what the heck"
date = 2026-07-24

[extra]
accent_color = "#426834"
accent_color_dark = "#A7D293"
banner = "banner.png"
card = "banner.png"
+++

So, a funny update to [chapter 0](https://asoulless.space/blog/home-server-ch0/): turns out my laptop battery is just fine??? I recently decided to poke around the BIOS settings to see if I could change any settings to reduce the impact on the battery while it would be plugged up. The BIOS menu was showing the battery as being about 95% charged, much higher than Plasma was claiming that it was at when it said it wasn't charging. Following that, I decided to boot into the system to see if I was still facing the same issue, and lo and behold, Plasma was telling me that the battery was, in fact, charging, and at around 95% or so, too. It strikes me as really odd, since not only was KDE Plasma telling me that the battery wasn't charging, but there was also a flashing amber light next to the charging point. Looking it up, [this support forum post](https://www.dell.com/community/en/conversations/inspiron/continuous-amber-flashing-of-battery-led-indicator/647f9864f4ccf8a8deb81612) caused me to believe that the battery was on the way out. Now, the light momentarily shines white before turning off. It's rather odd...

Despite that, I think I'm going to turn it into a server anyways. I might have to take some extra precautions to preserve the battery in the future. So, onto the main event:

# Moving to the light gaming computer
The first step to turning my school laptop into a server involved setting up my old gaming computer as my new personal device. I made a list of programs to install, configurations to migrate over, and other things to do to make sure that I didn't miss any steps and end up lamenting the fact that I didn't take my time. From there, I installed the same OS that was on that laptop, connected it to my home network, and connected it to my Tailnet. Over the next few days, I then installed the programs that I needed, connected the computer to my Nextcloud instance, and slowly set up each program, configuring settings and config files. The process actually went a bit quicker than I thought it would. However, that was just the easy part.

# Secure Boot and LUKS
I wanted to encrypt my hard drive, keeping a similar setup to what I had on my laptop. Sure, the chances of someone breaking into my home and stealing the hard drive, if not the entire system, were pretty low, but I prefer to do so regardless of the fact. The issue with this, however, was that, while my laptop had a (now somewhat finicky) keyboard, my desktop computer didn't have that. Add on the fact that I now mainly use a wireless keyboard, and having a LUKS-encrypted drive meant that every single time I rebooted the computer, I had to grab my old wired keyboard from the closet, plug it in, and use that to enter the passphrase. It wasn't a deal breaker for me, but I didn't really want the inconvenience. Or rather, I knew I could do something about it. So, I turned to using the TPM on the motherboard to decrypt the drive.

I looked up how to do it, learned a bit about how the process actually worked, and eventually followed [this guide](https://fedoramagazine.org/use-systemd-cryptenroll-with-fido-u2f-or-tpm2-to-decrypt-your-disk/) to get it up and running (though I originally followed [this guide](https://fedoramagazine.org/automatically-decrypt-your-disk-using-tpm2/) before learning that I only needed `systemd-cryptenroll` and not `clevis` for my system). Now automatic disk decryption was working. Somewhat. Sometimes the system would fail to pass the checks put in place by Secure Boot, causing it to prompt me for the passphrase, but sometimes it would just work. Even as I write this, it's still off and on, and I'm not entirely sure of how to get it to be consistent, even if I do have a better idea of it now. For now, it's something that I'll likely tinker with more in the future, but for now, it works.

Well, now that I've dealt with another issue:

# NVIDIA
I think it's safe to say that if you've been using Linux for a bit, you've heard of how much of a pain working with NVIDIA GPUs is on Linux. When I made the decision to switch to this computer, whose only way of getting graphics to the screen was the NVIDIA card (see the specs of the machine in the previous post; the CPU doesn't have an iGPU, a decision that I made when picking out the parts to save some money), I completely forgot about the sleeping issues that I had faced in the past when I had previously used various Linux distributions on the computer. So, when I first put the computer to sleep to see if it would wake back up, it worked fine, but not without visual artifacts. Restarting the system would fix the issue, so I just set the system up to avoid causing it to go to sleep as much as I could.

I eventually searched up the issue, and after trying a few config file changes suggested by others to no avail, I eventually found a Reddit post (that I'm failing to find now that I'm searching again lol) where one comment said that it looked like the GPU was dying. Worried, I eventually searched up "dying gpu", and ended up on [this article](https://pcgearhead.com/how-to-tell-if-a-gpu-is-dying/) that suggested updating the GPU driver. Sure, it was aimed at Windows users, but that caused me to look into checking the NVIDIA drivers, since I remembered messing around with the NVIDIA driver settings in the past on other Linux distributions. After some searching, I ended up on [this guide](https://rpmfusion.org/Howto/NVIDIA) for installing NVIDIA drivers on Fedora Linux. Perfect! Problem solved! I just needed to run some commands and-
> Secure Boot. If you have secure boot enabled in BIOS/EFI, you must sign the nvidia kmod. Either disable secure boot, or follow the [Secure Boot HowTo](https://rpmfusion.org/Howto/Secure%20Boot) to create and import your own generated secure boot key. Once done, kmod will be automatically signed with key. It is critical this is done before installing the kmod. Failure to do so will disable the driver and result in a blank screen.

Oh. Right. Makes sense that I had to sign the NVIDIA driver to make sure that the system would boot. I'm at least glad that I read that, because when I eventually failed to follow the Secure Boot instructions and ended up with a black screen with only a cursor, I wasn't caught off guard.

{% alert(warning=true) %}
The rest of this post is going to get a bit command-heavy. It's been a few days since I've gone through these steps, so if you're looking for help on trying to do something similar, please refer to the articles and guides that I've listed instead of following each of my steps verbatim. After all, this is more so a retelling of events than a guide.
{% end %}

## Secure Boot, now with MOKs!
This was probably the hardest part of getting the drivers, Secure Boot, and TPM decryption working. After failing the first time and getting that black screen, then making use of Proton's Lumo to try to understand what I *actually* needed to do, I eventually got things working. Here's what I had to do:

First, I had to install the needed tools (some of which were already on the system):
```bash
sudo dnf install kmodtool akmods mokutil openssl
```

Then, I had to generate an <abbr title="Machine Owner Key">MOK</abbr>:
```bash
sudo kmodgenca -a
```

The first time I did this, `kmodgenca` warned me that there was already a key there that it would've overwritten if it continued. Confused, I thought that that meant that `akmod` would have it available and ready to use. Not so; I still had to run the following step to enroll the MOK:
```bash
sudo mokutil --import /etc/pki/akmods/certs/public_key.der
```

This step also confused me when I first did it, too, not only because I accidentally skipped the previous step the first time, but also because, contrary to the instructions on the Secure Boot page and even the `README.secureboot` file that they point you to for more information, `mokutil` just asked me for a password, not to generate one like what was on the page. That led me to believe that it was asking me for a password that was already there, not for a new one. Lumo assured me that that was expected behaviour (consistent with [Oracle's guide on mokutil](https://docs.oracle.com/en/engineered-systems/exadata-database-machine/dbmsq/adding-keys-using-mokutil.html)), which ended up being the case. As a warning, when you do give it a password to use, generate something secure but easy to type, as you will have to type this in the very next time the system boots.

From there, I then needed to reboot the system, then go through the steps to enroll the MOK public key. Choose `Enroll MOK`, `Continue` (or, if you would like to see the key(s) that are already there, `View key 0`), then `Yes` to confirm that you want to continue. When prompted, enter that password/phrase that you gave `mokutil` earlier. Keep in mind, though, as the guide states,
> WARNING: keyboard is mapped to QWERTY!

Once the key is enrolled, you can tell the system to reboot.

## Finally. Working drivers.
Now back in the system, I could then continue with the steps to install the NVIDIA drivers:
```bash
sudo dnf update -y # and reboot if you are not on the latest kernel
sudo dnf install akmod-nvidia # rhel/centos users can use kmod-nvidia instead
```
The guide also lists a 3rd command here, but I didn't need it, since I wasn't interested in CUDA support.

From there, I just had to check a few times to make sure that `sudo modinfo -F version nvidia` gave me a number and not, `modinfo: ERROR: Module nvidia not found`, then I rebooted the system. Once the system was back on, I re-enrolled the TPM and rebuilt initramfs to try to make sure that auto-unlocking would work:
```bash
sudo systemd-cryptenroll --wipe-slot tpm2 --tpm2-device auto --tpm2-pcrs "[whatever PCRs you want to use; see the guide I linked earlier]" /dev/[your LUKS partition; ditto]
```
Now that Secure Boot and the drivers are working, the system now wakes up from sleep without artifacts. There's still a few issues, like the inconsistency of TPM decryption, or even that, if the monitor is displaying any other input than the desktop's, it simply won't display anything when I switch over to it, leading me to have to restart the monitor, plug out and in the cable, or in worst cases, restart the computer entirely. Nevertheless, it's a start, and I learned a few things from the process.

At this point, the system's pretty stable. I'm just sitting with it a bit longer to make sure that it's good to go before setting up the laptop as another server. That's where the next chapter will pick up.