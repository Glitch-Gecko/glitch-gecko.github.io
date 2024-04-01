---
layout: post
title: "NCL Practice Round Writeups: Password Cracking"
author: GlitchGecko
tagline: Writeups for the NCL practice round Cryptography challenges
tags:
- National Cyber League
- Cyber Skyline
- CTF
- Writeup
- Password cracking
- Hashcat
- Ophcrack
- NTLM Hash
- MD5
---

NCL practice round Password Cracking challenge writeups

---

Following a short break after this year's CCDC, I jumped back into the CTF grind and started studying Crypto and Binary Exploitation. I've been looking for an opportunity to utilize my new skills, and it just so happens that my Cyber Defense Club was looking to sign a team up for the National Cyber League.

It definitely seems like I've learned a lot, especially since at the time of writing this, I'm 8th place out of almost 8,000 participants in the practice round, with over 95% challenge completion.

To help others (mostly my team at Lewis University), I've decided to create writeups for each challenge.

The following is a list of each Password Cracking challenge:

## Cracking 1 (Easy)
#### Our analysts have obtained password dumps storing hacker passwords. After obtaining a few plaintext passwords, it appears that they overlap with the passwords from the rockyou breach. Can you crack them?

```
d9a336ea69220c9383bcf9ab0a86b13d
b48be5fe13b280e8da3f4eb092f663b1
fa19d2afdedbfe896ca1710ca2f46502
```

So first, I stored all these hashes in a file:

```sh
glitchgecko:/tmp/cybersky$ cat hashes           
d9a336ea69220c9383bcf9ab0a86b13d
b48be5fe13b280e8da3f4eb092f663b1
fa19d2afdedbfe896ca1710ca2f46502
```

From here, we can use `hashid -m hashes` to discover what each hash is along with the hashcat mode.

All hashes have the same result:
```
Analyzing 'fa19d2afdedbfe896ca1710ca2f46502'
[+] MD2 
[+] MD5 [Hashcat Mode: 0]
[+] MD4 [Hashcat Mode: 900]
[+] Double MD5 [Hashcat Mode: 2600]
[+] LM [Hashcat Mode: 3000]
```
(Note that I scrubbed some extra data from the bottom)

From this, we can tell that these are MD5 hashes, which can easily be cracked in Hashcat:

`hashcat -m 0 -O -a 0 -D 1 -w 3 hashes /usr/share/wordlists/rockyou.txt`

Here's an explanation for each flag I used:

| Flag | Description |
| --- | --- |
| `-m 0` | Sets the hashcat mode to MD5 cracking |
| `-O` | Optimizes kernel |
| `-a 0` | Attacks using wordlist (dictionary attack) |
| `-D 1` | Forces GPU usage |
| `-w 3` | Sets workload profile |

This should crack each password! Note that if you've already cracked them, you can use `--show` and see the cracked passwords:

```sh
glitchgecko:/tmp/cybersky$ hashcat -m 0 -O -a 0 -D 1 -w 3 hashes /usr/share/wordlists/rockyou.txt --show
d9a336ea69220c9383bcf9ab0a86b13d:<censored_password>
b48be5fe13b280e8da3f4eb092f663b1:<censored_password>
fa19d2afdedbfe896ca1710ca2f46502:<censored_password>
```

## Cracking 2 & 3 (Easy)
#### Description: "Our analysts have obtained password dumps storing hacker passwords. Try your hand at cracking them."

```
EAE8BD72421771461AA818381E4E281B:84C04080BE83F8770208590FE3032CA6
B61FEBA7B3C5F0BB1AA818381E4E281B:7925FAD4F4AB8292CB8DE49AC527197E
AD6C338B0C2F8CBC1AA818381E4E281B:B957AE9E916BE00F7497F4FA10CF139B
40F967490D91A797C2265B23734E0DAC:5DCD6A9AA5D7E4D4064FB34160B27DD9
8D04A4C8BBA0DD3B9C5014AE4718A7EE:BD39C2FF33AE1D75D37498D026CD83E1
D90707D7CA404EE025AD3B83FA6627C7:C29AE3BC6DEF554C1A2C4123FE8CCD29
```

I've combined the two of these challenges into one heading because the solve method is exactly the same for both, and they even have the same description.

As before, I've stored these hashes into a temporary hash file. For this one, I did some research on password dumps:

![Google Search](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/search2.png)

Here I found that password dumps like these are likely to be SAM hashes, which are commonly cracked using [Ophcrack](https://ophcrack.sourceforge.io/).

To use Ophcrack, you also have to download a rainbow table for cracking. I personally used the "XP free fast" table found [here](https://ophcrack.sourceforge.io/tables.php) on the Ophcrack page.

After loading the passwords and table into Ophcrack, just press the `Crack` button to begin cracking! It shouldn't take too long, as it only took 5 seconds to crack all 6 passwords on my laptop.

![ophcrack](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/ophcrack.png)

## Cracking 4 (Medium)
#### Description: "Our analysts have obtained a database dump. See if you can retrieve the admin password."
#### Provided Files: [wp561.sql](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/wp561.sql)

`Q1: What is the username of the admin?`

`Q2: What is the hash of the admin password?`

Here we're given a SQL script that builds up a database. Searching for the `user` keyword finds the following lines:
```sql
INSERT INTO `wpbh_users` (`ID`, `user_login`, `user_pass`, `user_nicename`, `user_email`, `user_url`, `user_registered`, `user_activation_key`, `user_status`, `display_name`) VALUES
(1, 'carter', '$P$BVkil0p850ufCHs.ovF/0eS96iY/hz1', 'carter', 'carter@carterparsons.com', '', '2018-10-02 19:29:53', '', 0, 'carter');
```

Using this information, we can answer both questions 1 and 2.

### Q3: What is the plaintext of the admin's password?

For this, we can once again use `hashid` to determine the hash type:

```sh
nicolas:/tmp/cybersky:$ hashid -m '$P$BVkil0p850ufCHs.ovF/0eS96iY/hz1'                           
Analyzing '$P$BVkil0p850ufCHs.ovF/0eS96iY/hz1'
[+] Wordpress ≥ v2.6.2 [Hashcat Mode: 400]
[+] Joomla ≥ v2.5.18 [Hashcat Mode: 400]
[+] PHPass' Portable Hash [Hashcat Mode: 400]
```

Knowing this, we can easily crack the hash just like before:
`hashcat -m 400 -O -a 0 -D 1 -w 3 '$P$BVkil0p850ufCHs.ovF/0eS96iY/hz1' /usr/share/wordlists/rockyou.txt`

## Unsolved

Unfortunately, these two challenges are the only two in the whole practice round that I have not yet solved. I'm leaving these here in case anyone else wants to take a crack at them (pun intended), and I'll try to come back at a future date with a solution.

### Cracking 5 (Hard)
#### Description: "Our analysts have obtained password dumps storing hacker passwords, they all seem to be the same theme. Can you figure them out?"

```
b984d8596da227068836b45a505379c2
637ef0055228d85e8217fcdec7bfd639
9831671703e692e36d22e1ace91cb75d
a2b44565e62f6e979beb05ebf1acbbd2
9374abc3bfbd5853dc9d5bc6e51a9ef9
```

### Cracking 6 (Hard)
#### Description: "Our analysts have obtained password dumps storing hacker passwords. We know many of them enjoy traveling, maybe their passwords reflect that."

```
$1$xmc$yobfnsJdOGiPN85SA8eiT/
$1$dac$vvespCH9zjky8oxvq1nel/
$1$erj$JgjXnThWvnEKYBZJY1CP./
$1$axu$351SxqKtW7CgjWvVAzuca1
```

# Final thoughts

Even though this was just the practice round, I had a lot of fun working my way through the challenges! I'm very happy with my noticable skill improvement, and I'm definitely excited to see how the individual and team rounds go.

Make sure to check out my other writeups!

![Scorecard](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/score.png)