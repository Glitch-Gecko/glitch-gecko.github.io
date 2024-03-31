---
layout: post
title: "NCL Practice Round Writeups: OSINT"
author: GlitchGecko
tagline: Writeups for the NCL practice round OSINT challenges
tags:
- National Cyber League
- Cyber Skyline
- CTF
- Writeup
- OSINT
- Open source intelligence
---

NCL practice round OSINT challenge writeups

---

Following a short break after this year's CCDC, I jumped back into the CTF grind and started studying Crypto and Binary Exploitation. I've been looking for an opportunity to utilize my new skills, and it just so happens that my Cyber Defense Club was looking to sign a team up for the National Cyber League.

It definitely seems like I've learned a lot, especially since at the time of writing this, I'm 8th place out of almost 8,000 participants in the practice round, with over 95% challenge completion.

To help others (mostly my team at Lewis University), I've decided to create writeups for each challenge.

The following is a list of each OSINT challenge:

## Hidden (Easy)
#### Description: "We suspect that hackers have encoded intel into this image. Analyze it and see what you can find out."

#### Provided files: [Hidden.jpeg](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/Hidden.jpeg)

`Q1: What is the name of the creator of the image?`

`Q2: What camera was this photo taken with?`

`Q3: What is the flag stored in the image?`

For this challenge, you can solve all three questions with just one easy command: `exiftool Hidden.jpeg`. Exiftool is a useful CLI tool that allows you to easily see image metadata. Scrolling through the output, we can easily find the answers we're looking for:
```
Make                            : Hacker Vision
Camera Model Name               : Hackercam 2000
Artist                          : M@st3r Hackr
User Comment                    : FLAG: SKY-EXIF-1890
```
## WHOIS (Easy)
#### Description: "Conduct open source intelligence data collection about cityinthe.cloud. Answer the following questions as they relate to the cityinthe.cloud domain."

`Q1: Who is the registrar of this domain?`

`Q2: On what day was this domain first registered?`

`Q3: What is this domain's registry domain id?`

The above questions can all be answered using `whois cityinthe.cloud`, which queries domain information:
```
Registry Domain ID: D15CD1AC4DEB54207A5048A69B9FC0558-ARI
Registrar WHOIS Server: whois.dynadot.com
Creation Date: 2016-02-16T18:23:14.904Z
Registrar: Dynadot, LLC
```

### Q4: What is the TLD of this domain?

This one doesn't need any tools. The TLD is simply the last part of the domain, following the domain name:

cityinthe.`cloud`

### Q5: What organization manages the TLD used by cityinthe.cloud?

This one only needed a quick google search:
![Search](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/search.png)

## Hacker Location (Medium)
#### Description: "We have been getting chatter from a group of malicious hackers setting up a meeting point. We believe these weird sets of 3 words describe where they are meeting. Can you locate them?"

```
corded nearing heave

flipping cages guru

chuckle breathed defends
```

This one was actually pretty easy. A quick google search takes you to the following site, which pretty much has the answer:

[https://what3words.com/corded.nearing.heave](https://what3words.com/corded.nearing.heave)

I'm sure you can do the rest from there.

`Q1: What is the name of the city where the meeting is planned?`

`Q2: What is the name of the building where the meeting is planned?`

## Shopping List (Medium)
#### Description: "Tom is very forgetful so he started a shopping list for one of the stores he needs to go to this weekend, but he didn't write down which store, can you find out what Tom was looking to buy?"

`Q1: What is the item for 1.961.01?`

`Q2: What is the item for 300.557.6?`

`Q3: What is the item for 3.011.35?`

Admittedly, I was a little stumped on this one. After consulting ChatGPT for advice, I ~~realized~~ was told that these resembled IKEA article numbers. I found IKEA's [search page](https://www.ikea.com/us/en/search/) and began searching. Unfortunately, this was a fruitless endeavour, and didn't find anything. 

I began looking at other products, and realized that the article numbers were always 8 digits long, formatted as `[3 digits].[3 digits].[2 digits]`. This meant that the items given are missing some digits.

I added the most likely padding to all three and searched the following:
```
Item 1: 001.961.01
Item 2: 300.557.60
Item 3: 003.011.35
```

I found that two of the items were actually discontinued, but a quick google search of the name found what it was anyways.

## Geolocation (Hard)
#### Description: "We have seized flash drives from some criminals with images of what appears to be a facility that they are targeting. Can you review the images and figure out what their target was? Remember, you cannot ask anyone to help locate this, you have to use openly available information online to find this location."
#### Provided files:
![front](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/Front.png)
![back](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/Back.png)

This one was pretty much just a GeoGuesser challenge.

Without knowing the building, looking at the 'front' image tells you that you're on [US 31W](https://en.wikipedia.org/wiki/U.S._Route_31W). The 'back' image tells you you're near somewhere ending in 'ville'.

The wikipedia page shows a couple 'ville's, but looking under the Kentucky part, you see that the route passes through Fort Knox within sight of the US Bullion Depository by Louisville.

From there, you just have to find the address of the depository.

# Final thoughts

Even though this was just the practice round, I had a lot of fun working my way through the challenges! I'm very happy with my noticable skill improvement, and I'm definitely excited to see how the individual and team rounds go.

Make sure to check out my other writeups!

![Scorecard](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/score.png)