---
layout: post
title: "How I hacked my highschool laptop"
tagline: Why physical security is just as important as cybersecurity
author: GlitchGecko
tags:
- Hardware Hacking
- Writeup
- Physical Security
- BIOS
---

In this writeup, I'll be sharing my experience with repurposing a high-school laptop that I got from a recent grad. My goal was pretty straightforward: set up a few servers for my home network, like a pihole, an image-hosting server, and a password manager. But, there was a catch – the laptop was locked down to only work with school network accounts. Challenge accepted!

![HardwareHacking 1](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/HardwareHacking1.jpg)

## The Challenge
So, I plugged in the laptop, powered it up, and... hit a wall. I couldn't log in because the school had restricted access to their network accounts only. I tried a couple of things right away: brute-forcing the admin password, reaching out to a friend who used to work in the school's IT (but he only had the Chromebook admin password, not the Windows one 😔), and even attempting a factory reset. No dice. The school had also locked down system configurations and BIOS access, preventing any exploitation from using a boot device or factory reset.

![HardwareHacking 2](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/HardwareHacking2.jpg)

Eventually, I found [bios-pw.org](https://bios-pw.org), a site for obtaining bios master passwords.

## The Solution
Unfortunately, the password from the website did not work. This led me to investigate the hardware side of things to look for possible ways to physically alter the device to gain access.

I discovered that it's possible to reset a BIOS password by simply draining the CMOS battery, which holds BIOS information. Upon discovering this, I quickly started disassembling the laptop.

![HardwareHacking 3](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/HardwareHacking3.jpg)
![HardwareHacking 4](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/HardwareHacking4.jpg)

![HardwareHacking 5](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/HardwareHacking5.jpg)

The image above shows the CMOS battery (the circular battery with red and black wires). I unplugged it, powered on the laptop, and then shut it down again, which drained the battery.

After draining the battery, I reset the password using [bios-pw.org](https://bios-pw.org) and finally got full control of the laptop. This allowed me to change the boot order and install whatever I wanted.

![HardwareHacking 6](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/HardwareHacking6.jpg)

## Conclusion
Hardware hacking turned out to be a winning strategy for unlocking the high-school laptop. By getting hands-on with the device and exploring different approaches, I managed to bypass the restrictions to load up an Arch Linux boot ISO. Though I didn't end up using it for the servers, I still use the laptop for CTFs and network tests.

This writeup can serve as a word of caution: BIOS access is quite dangerous, because in the wrong hands, it could be used for installing malware, booting from a malicious device, or stealing sensitive data. 

Thanks for reading!