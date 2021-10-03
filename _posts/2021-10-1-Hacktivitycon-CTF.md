---
layout: post
title: "H@cktivitycon CTF Writeups"
author: Ainchentmew2
---

Note: I'm still cleaning up the site so this is unfinished as of now.

# H@cktivitycon CTF

Before now, I've had no experience in anything involving Cybersecurity, but when I heard about the CTF run by John Hammond, congon4tor, M_alpha, fumenoid, NightWolf, Blacknote, and CalebStewart, I figured it'd be a great learning experience for me.


I spent several hours researching and learning, in hopes of building up my abilities before the CTF. When it came time to start, I felt like I'd be a little useless and not have much to bring to the team, but it turned out that I actually solved seven challenges, netting  me just over 1100 points.


In this post, I'm going to be detailing my thought process and how I found each flag, based on the order I completed them in.

## Tsunami
#### Description: "Woah! It's a natural disaster! But something doesn't seem so natural about this big wave...

Nobody on my team seemed interested in trying this one, as it involved an audio file and none of us had headphones.
Checking the file type, I found that "tsunami" was a .wav file, so I renamed it accordingly.

![image](https://github.com/Ainchentmew2/ainchentmew2.github.io/blob/main/images/Tsunami2.png)

I figured maybe there was something hidden in the visualization in the wave itself, so I downloaded it and opened it in sonic visualiser.

![image](https://github.com/Ainchentmew2/ainchentmew2.github.io/blob/main/images/Tsunami3.png)

I couldn't find anything right away, but I looked at the spectrogram view and zoomed in a little towards the end, and the flag was right there in plaintext!

![image](https://github.com/Ainchentmew2/ainchentmew2.github.io/blob/main/images/Tsunami4.png)

## Bass 64
#### Description: "It, uh... looks like someone bass-boosted this? Can you make any sense of it?"

Downloading this file resulted in an ASCII text file with every letter represented as ASCII art. Based on the name and the hint, I assumed that it was encoded in base 64. Sure enough, I copied down the letters into a base 64 decoder, and got the flag!

## Target Practice
#### Description: "Can you hit a moving target?"

This one was a gif that looked a little like a target with vibrating dots on the outside of the target. I split apart the frames of the gif, and noticed that frame 16 looked a little odd compared to the rest, based on the top left corner sticking out. I wondered what I could do with the image, and realized it looked a little like a QR code. Upon searching for circular QR codes, I found out that the image was actually a Maxi code, and scanning it gave the flag!

## Butter Overflow
#### Description: "Can you overflow this right?"

