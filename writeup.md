---
layout: default
---
Note: I'm still cleaning up the site so this is unfinished as of now.

# H@cktivitycon CTF

Before now, I've had no experience in anything involving Cybersecurity, but when I heard about the CTF run by John Hammond, congon4tor, M_alpha, fumenoid, NightWolf, Blacknote, and CalebStewart, I figured it'd be a great learning experience for me.

I spent several hours researching and learning, in hopes of building up my abilities before the CTF. When it came time to start, I felt like I'd be a little useless and not have much to bring to the team, but it turned out that I actually solved seven challenges, netting  me just over 1100 points.

In this post, I'm going to be detailing my thought process and how I found each flag, based on the order I completed them in.

## Tsunami
#### Description: "Woah! It's a natural disaster! But something doesn't seem so natural about this big wave...

Nobody on my team seemed interested in trying this one, as it involved an audio file and none of us had headphones. I figured maybe there was something hidden in the visualization in the wave itself, so I downloaded it and opened it in sonic visualiser. I couldn't find anything right away, but I looked at the spectrogram view and zoomed in a little towards the end, and the flag was right there in plaintext!

## Bass 64
#### Description: "It, uh... looks like someone bass-boosted this? Can you make any sense of it?"

Downloading this file resulted in an ASCII text file with every letter represented as ASCII art. Based on the name and the hint, I assumed that it was encoded in base 64. Sure enough, I copied down the letters into a base 64 decoder, and got the flag!

## Target Practice
#### Description: "Can you hit a moving target?"

This one was a gif that looked a little like a target with vibrating dots on the outside of the target. I split apart the frames of the gif, and noticed that frame 16 looked a little odd compared to the rest, based on the top left corner sticking out. I wondered what I could do with the image, and realized it looked a little like a QR code. Upon searching for circular QR codes, I found out that the image was actually a Maxi code, and scanning it gave the flag!

## Butter Overflow
#### Description: "Can you overflow this right?"

Upon connecting to the port, you're asked a simple question: "How many bytes does it take to overflow this buffer?"

I did what any sane person would do, and proceded to hold the letter 'a' for quite some time. I don't know exactly how many I needed to overflow the buffer, but this worked and got the flag!
![Overflow](https://github.com/Ainchentmew2/ainchentmew2.github.io/blob/main/images/Butter_Overflow2.png)

## Integrity
#### Description: "My school was trying to teach people about the CIA triad so they made all these dumb example applications... As if they know anything about information security. Supposedly they learned their lesson and tried to make this one more secure. Can you prove it is still vulnerable?"
