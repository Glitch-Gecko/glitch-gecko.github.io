---
layout: post
title: "NCL Practice Round Writeups: Network Traffic Analysis"
author: GlitchGecko
tagline: Writeups for the NCL practice round Network Traffic Analysis challenges
tags:
- National Cyber League
- Cyber Skyline
- CTF
- Writeup
- Network Traffic Analysis
---

NCL practice round Network Traffic Analysis challenge writeups

---

Following a short break after this year's CCDC, I jumped back into the CTF grind and started studying Crypto and Binary Exploitation. I've been looking for an opportunity to utilize my new skills, and it just so happens that my Cyber Defense Club was looking to sign a team up for the National Cyber League.

It definitely seems like I've learned a lot, especially since at the time of writing this, I'm 8th place out of almost 8,000 participants in the practice round, with over 95% challenge completion.

To help others (mostly my team at Lewis University), I've decided to create writeups for each challenge.

The following is a list of each Network Traffic Analysis challenge:

## HTTP (Easy)
#### Description: "Analyze HTTP traffic to extract a flag."
#### Provided files: [HTTP.pcapng](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/HTTP.pcapng)

Q1 - 20 points
What is the name of the server as indicated by the "server" header in the HTTP responses?
Simple HTTP
	
Q2 - 20 points
What is the filename of the image containing the flag?
2lj07p.jpg
	
Q3 - 30 points
In what packet number is the request to get the image containing the flag made?
50
	
Q4 - 35 points
What is the flag contained in the downloaded image?
SKY-MXUN-4558

## DNS (Medium)
#### Description "Analyze the raw data from a DNS packet. Consider researching the DNS packet structure."

```
0000   000c 29a1 c3d0 0050 56f3 edb8 0800 4500
0010   0053 d935 0000 8011 a590 c0a8 1d02 c0a8
0020   1d81 0035 abbd 003f 394d 21d1 8180 0001
0030   0001 0000 0001 0667 6f6f 676c 6503 636f
0040   6d00 0001 0001 c00c 0001 0001 0000 0005
0050   0004 acd9 0d4e 0000 2910 0000 0000 0500
0060   00
```

Q1 - 15 points
What is the IP address of the DNS resolver?
192.168.29.2
	
Q2 - 15 points
What was the client's source port was used to establish the connection with the resolver for the DNS query?
43965
	
Q3 - 15 points
What domain was queried?
google.com
	
Q4 - 15 points
What is the IP address of the domain that was queried?
172.217.13.78

## WiFi (Hard)
#### Description: "A wireless network was hacked. See if you can figure out what happened."
#### Provided files: [wifi.pcapng](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/wifi.pcapng)

Q1 - 10 points
What is the SSID of the network?
Nacho Wifi
	
Q2 - 10 points
What is the MAC Address of the router?
9C:D3:6D:AE:52:4A

Q3 - 10 points
What is the MAC Address of the client?
00:DB:DF:D2:4D:DF
	
Q4 - 10 points
What channel is the router operating on?
11
	
Q5 - 15 points
What is the name of the 802.11i cipher algorithm being used by the router?
AES
	
Q6 - 15 points
What security technique, introduced in the 802.11i standard, is displayed in the capture?
Four way handshake
	
Q7 - 50 points
What is the password?
tiPperpAblo

# Final thoughts

Even though this was just the practice round, I had a lot of fun working my way through the challenges! I'm very happy with my noticable skill improvement, and I'm definitely excited to see how the individual and team rounds go.

Make sure to check out my other writeups!

![Scorecard](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/score.png)