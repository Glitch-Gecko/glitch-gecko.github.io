---
layout: post
title: "NCL Practice Round Writeups: Log Analysis"
author: GlitchGecko
tagline: Writeups for the NCL practice round Log Analysis challenges
tags:
- National Cyber League
- Cyber Skyline
- CTF
- Writeup
- Log Analysis
---

NCL practice round Log Analysis challenge writeups

---

Following a short break after this year's CCDC, I jumped back into the CTF grind and started studying Crypto and Binary Exploitation. I've been looking for an opportunity to utilize my new skills, and it just so happens that my Cyber Defense Club was looking to sign a team up for the National Cyber League.

It definitely seems like I've learned a lot, especially since at the time of writing this, I'm 8th place out of almost 8,000 participants in the practice round, with over 95% challenge completion.

To help others (mostly my team at Lewis University), I've decided to create writeups for each challenge.

The following is a list of each Log Analysis challenge:

## Event Logs (Easy)
#### Description: "Analyze a Windows event log to identify Windows Update behavior."
#### Provided files: [Windows_Update.csv](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/Windows_Update.csv)
Q1 - 10 points
What time was the system rebooted? (round to the nearest minute)
2020-01-19 21:23
	
Q2 - 15 points
How many "Error" level events are logged?
5
	
Q3 - 15 points
How many times does the Microsoft-Windows-Kernel-Power Source record the system entering sleep?
69
	
Q4 - 20 points
When was the last Candy Crush Saga update installed? (round to the nearest minute)
2020-01-30 11:50
	
Q5 - 25 points
How many security patches were installed on the machine?
50
	
Q6 - 25 points
What was the maximum number of patches found by Windows Update in a single check?
17

## Hiding (Medium)
#### Description: "Investigate an S3 bucket log."
#### Provided files: [ads_website.log](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/ads_website.log)

Q1 - 15 points
What operation type does not have a listed requester?
website.get.object
	
Q2 - 15 points
How many different types of operations are listed in the log?
21
	
Q3 - 15 points
How many unique IP addresses made requests?
40
	
Q4 - 25 points
How many requests were made from the desktop version of Safari?
169
	
Q5 - 25 points
What was the most common request URI that caused 404 errors?
/hegvd-ads?policy

	
Q6 - 25 points
What is the average amount of time that requests spent in flight? (round up to the nearest millisecond)
46

## Deleted Flag (Hard)
#### Description: "Someone was scraping government websites. We managed to obtain the log file from their scraper. Investigate what websites they were targeting."
#### Provided files: [wpull.log](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/wpull.log)

Q1 - 10 points
How many files were downloaded?
76795
	
Q2 - 15 points
How many warnings were generated?
1534
	
Q3 - 15 points
How many requests returned 200 status codes?
76790
	
Q4 - 15 points
How many images were successfully fetched with 200 status codes?
15876
	
Q5 - 25 points
How many different reasons were given for wpull errors?
6
	
Q6 - 25 points
How many different domains yielded errors? (treat each subdomain as unique)
23
	
Q7 - 45 points
How many unique .gov domains were "Fetched" both successfully and unsuccessfully? (treat each subdomain as unique)
131

# Final thoughts

Even though this was just the practice round, I had a lot of fun working my way through the challenges! I'm very happy with my noticable skill improvement, and I'm definitely excited to see how the individual and team rounds go.

Make sure to check out my other writeups!

![Scorecard](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/score.png)