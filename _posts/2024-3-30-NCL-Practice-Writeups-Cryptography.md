---
layout: post
title: "NCL Practice Round Writeups: Cryptography"
author: GlitchGecko
tagline: Writeups for the NCL practice round Cryptography challenges
tags:
- National Cyber League
- Cyber Skyline
- CTF
- Writeup
- Cryptography
- Crpyto
- RSA
- Encoding
- Cyberchef
---

NCL Practice Round Crypto challenge writeups

---

Following a short break after this year's CCDC, I jumped back into the CTF grind and started studying Crypto and Binary Exploitation. I've been looking for an opportunity to utilize my new skills, and it just so happens that my Cyber Defense Club was looking to sign a team up for the National Cyber League.

It definitely seems like I've learned a lot, especially since at the time of writing this, I'm 8th place out of almost 8,000 participants in the practice round, with over 95% challenge completion.

To help others (mostly my team at Lewis University), I've decided to create writeups for each challenge.

The following is a list of each Crypto challenge:

## Decoding 1 (Easy)
#### Description: "Our analysts have obtained an encrypted message. See if you can decode it."

`India Hotel-Alpha-Victor-Echo Sierra-India-Xray Mike-Echo-Tango-Echo-Romeo-Papa-Romeo-Echo-Tango-Echo-Romeo Sierra-Echo-Sierra-Sierra-India-Oscar-November-Sierra`

This one shouldn't be terribly hard. Anyone familiar with the phonetic alphabet should immediately get this one, but otherwise, it's easy to see that some words are grouped by dashes.

Take the first letter of each word to create the plaintext:

`I`ndia `H`otel-`A`lpha-`V`ictor-`E`cho `S`ierra-`I`ndia-`X`ray `M`ike-`E`cho-`T`ango-`E`cho-`R`omeo-`P`apa-`R`omeo-`E`cho-`T`ango-`E`cho-`R`omeo `S`ierra-`E`cho-`S`ierra-`S`ierra-`I`ndia-`O`scar-`N`ovember-`S`ierra

Now following the groupings, you can easily decode the message and get the flag.

## Decoding 2 (Easy)
#### Description: "Our analysts have obtained some encrypted messages. See if you can decode them."

### Q1: Gurer jrer gra ubfgf hc va gur cbeg fpna

In general, if a sentence looks like it could be legitimate words but is gibberish with letters like the above, it's probably some sort of basic cipher like the Caesar (rot13) cipher.

You can use [Cyberchef](https://gchq.github.io/CyberChef/#recipe=ROT13(true,true,false,13)&input=R3VyZXIganJlciBncmEgdWJmZ2YgaGMgdmEgZ3VyIGNiZWcgZnBuYQ&oenc=65001) to easily decode stuff like this. Note that for more complicated ciphers, [dcode.fr](https://www.dcode.fr/en) might be a little better.

### Q2: Y vekdt uywxj TDI husehti veh jxqj tecqyd

This one can't be decoded with just a rot13, so there seems to be some other value. You can use the [rot13 brute force option](https://gchq.github.io/CyberChef/#recipe=ROT13_Brute_Force(true,true,false,100,0,true,'')&input=WSB2ZWtkdCB1eXd4aiBUREkgaHVzZWh0aSB2ZWgganhxaiB0ZWNxeWQ&oenc=65001) on cyberchef to find the flag.

## Decoding 3 (Easy)
#### Description: "Our analysts have obtained an encrypted message. See if you can decode it."

`What is the plaintext of the message: 8-44-33 8-2-777-4-33-8 44-2-7777 2 4-33-666-333-33-66-222-33 2-555-2-777-6?`

I'm sure anyone who has been around long enough to have used a cellphone keypad can tell that this is a Multi-Tap Phone cipher. The following image can be used as a reference if you don't have a cellphone on hand:

![keypad](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/keypad.png)

Alternatively, you can use [dcode's multitap decoder](https://www.dcode.fr/multitap-abc-cipher), but just make sure to remove all dashes for your input:
`84433 827774338 4427777 2 433666333336622233 255527776`

## Decoding 4 (Medium)
#### Description: "We have found an encrypted flag. See if you can decrypt it."

`QlpoOTFBWSZTWb71818AAAUeAAACQQAAjEogIAAxADAgZDJrMA8gyn4u5IpwoSF96+a+`

This one shows off a cool feature of cyberchef. When you paste the encrypted flag into the input, a magic wand icon appears next to the output header. This means that cyberchef has detected a probable encoding.

Simply click the magic wand to decode the flag.

Note that it may not necessarily be one and done; you might have to perform multiple operations.

## Decoding 5 (Hard)
#### Description: "Data was exfiltrated from a secured, air-gapped network with a series of QR codes. Investigate the evidence and see what you can find out."
#### Provided files: [exfil.tar.gz](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/exfil.tar.gz)

### Q1: How many QR codes were used to exfiltrate the file?

After extracting the archive, you can easily use `ls -1 | wc` to see how many files are inside of the archive.

### Q2: Each QR code is storing part of the original file, what is the largest amount (in bytes) of the exfiltrated file being stored in a single QR code?

For this, we're going to have to scan the QR codes. I saw that some people were writing python scripts for this challenge, but I felt that I could get it done faster manually.

Because each image is in alphabetical order, I could easily use `zbarimg *` to get the output of each QR code. I found the byte amount per code by just counting.

### Q3: Reassemble the original file using the QR codes. What is the flag you find when you do?

To reassemble the QR code, I had to cut out the `QR-Code:` part at the beginning of each line. I did this using `zbarimg * | cut -b 9-27 > file`, which only stored the desired bytes into the file.

Analyzing the text, I realized it looked like base64 encoding. I imported the file into cyberchef and decoded base64 to find my suspicions confirmed as I received a compressed file.

Unzipping this file finds a router log, where you can find the Flag and a hash.

### Q4: It seems like there is also a password hash in the file. What is the plaintext password?

There's a few ways to crack the hash found in this log `$1$v1iC$weNfRI6ClLJkZtuh2htA91`. I personally went with using hashcat+rockyou.txt for this.

If you're ever unsure of what mode to use for hashcat or what kind of hash you're looking at, remember to use `hashid -m [hash]` to find that info. (e.g `hashid -m '$1$v1iC$weNfRI6ClLJkZtuh2htA91'`)

## Decoding 6 (Hard)
#### Description: "Our analysts have obtained several artifacts from a message that was encrypted with RSA. We need you to decrypt the message and figure out what the hackers are up to."

```
n = 485
e = 53
c = 153 75 309 310 74 203 208 401 310 371 363 451 125
```

I was very excited to see this challenge, as I've just recently been completing the [Cryptohack RSA Challenges](https://cryptohack.org/user/glitchgecko/), where I've learned to use and exploit RSA.

### Q1: What is the value of p (the smaller prime)?

As `N` is quite small, it's trivial to factor. Alternatively (even for large numbers), refer to [FactorDB](http://factordb.com/index.php?query=485).

### Q2: What is the value of q (the larger prime)?

FactorDB (or your method of choice) should also answer this.

### Q3: What is the plaintext of the encrypted message?

Unfortunately, we have to decode each byte at a time. This is done trivially using [dcode](https://www.dcode.fr/rsa-cipher), or you can also write a script (I didn't, it was easy enough to do it by hand).

## Image (Medium)
#### Description: "We have downloaded an image from a Russian forum, but cannot open it. Can you take a look and find it's contents? We saved the curl log - maybe that will help you."
#### Provided files: [Image.png](https://github.com/Glitch-Gecko/glitch-gecko.github.io/blob/main/post_assets/NCL_Practice/Image.png)

```
HTTP/1.1 200 OK
Server: nginx
Date: Tue, 24 Jan 2020 20:47:17 GMT
Content-Type: text/plain; charset=utf-8
Content-Length: 3015
Content-Encoding: br
Connection: keep-alive
X-XSS-Protection: 1; mode=block
X-Content-Type-Options: nosniff
X-Frame-Options: SAMEORIGIN
Referrer-Policy: strict-origin-when-cross-origin
Vary: Accept, Accept-Encoding
```

### Q1: What is the full name of the content encoding scheme being used on this image?

If you aren't familiar with "`br`" content encoding, a quick google search finds that `br` refers to the lossless data compression algorithm Brotli.

### Q2: What is the flag hidden in the image?

Knowing that it's brotli encoded, we can use `brotli --decompress` to decompress the image.

Before we do that, we first have to rename the image to `Image.br`.

Now we can run `brotli --decompress Image.br` and open it to read the flag!

# Final thoughts

Even though this was just the practice round, I had a lot of fun working my way through the challenges! I'm very happy with my noticable skill improvement, and I'm definitely excited to see how the individual and team rounds go.

Make sure to check out my other writeups!

![Scorecard](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/score.png)