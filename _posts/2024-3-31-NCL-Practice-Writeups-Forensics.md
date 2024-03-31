---
layout: post
title: "NCL Practice Round Writeups: Forensics"
author: GlitchGecko
tagline: Writeups for the NCL practice round Forensics challenges
tags:
- National Cyber League
- Cyber Skyline
- CTF
- Writeup
- Forensics
- Zlib
- Excel
- Magic Bytes
- Autopsy
---

NCL practice round Forensics challenge writeups

---

Following a short break after this year's CCDC, I jumped back into the CTF grind and started studying Crypto and Binary Exploitation. I've been looking for an opportunity to utilize my new skills, and it just so happens that my Cyber Defense Club was looking to sign a team up for the National Cyber League.

It definitely seems like I've learned a lot, especially since at the time of writing this, I'm 8th place out of almost 8,000 participants in the practice round, with over 95% challenge completion.

To help others (mostly my team at Lewis University), I've decided to create writeups for each challenge.

The following is a list of each Forensics challenge:

## Excellent Tracking (Easy)
#### Our spreadsheet to hold a flag has been tampered with. Can you find the true flag?
#### Provided Files: [flag_sheet.xlsx](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/flag_sheet.xlsx)

`Q1: What is the full name of the creator of the spreadsheet?`

`Q2: What is the full name of the person who tampered with the file?`

Because Microsoft Office are actually archives of files, we can actually decompress it using `7z` to extract each file inside:

```sh
nicolas:/tmp/cybersky:$ 7z x flag_sheet.xlsx
nicolas:/tmp/cybersky:$ ls
'[Content_Types].xml'   docProps   flag_sheet.xlsx   _rels   xl
```

The `docProps/core.xml` file usually contains information on file creation and modification:

```sh
nicolas:/tmp/cybersky:$ cat docProps/core.xml 
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<cp:coreProperties <dc:creator>Jack Jillian</dc:creator><cp:lastModifiedBy>Bobert Jones</cp:lastModifiedBy></cp:coreProperties>
```

Note that I've scrubbed some unnecessary data to make the output more readable. Also note that the time shown in this file is incorrect.

### Q3: What date and time was the file tampered with? (in the tamperer's local time)

Looking in `xl/revisions/revisionHeaders.xml`, we can see the revision log for the file. Here we can see the following entry:
```xml
<header guid="{6A4C214D-43A2-4C47-83AC-886AA643AF34}" dateTime="2021-01-05T08:43:50" maxSheetId="3" userName="Bobert Jones" r:id="rId2" minRId="1">
<sheetIdMap count="2"><sheetId val="1"/>
<sheetId val="2"/>
</sheetIdMap>
</header>
```

Here we can see that the second revision was done at `2021-01-05T08:43:50`.

### Q4: What is the flag?

For this one, opening the excel sheet shows a flag `SKY-EXCL-[4 numbers]`. Unfortunately, we know that this sheet has been modified, which means this flag is incorrect.

Since we know that the second revision was the malicious change, we can simply read the `xl/revisions/revisionLog2.xml` file.

`<oc r="A1"><v>4839</v></oc><nc r="A1"><f>FLOOR(RAND()*(9999-1000)+1000,1)</f></nc>`

Here we can see that `4839` was changed to a random number generator.

Using this, we can assume that the original flag is `SKY-EXCL-4839`.

## Hiding (Medium)
#### Description: "A hacker is allegedly hiding some important information, but to us this file just looks like garbage. Can you figure out their secret?"
#### Provided files: [file.raw](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/file.raw)

### What is the flag?

If we use binwalk on the file, we find a raw deflate compression stream at the first byte of the file:

```sh
nicolas:/tmp/cybersky:$ binwalk -X file.raw 

DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             Raw deflate compression stream
```

This means that the file is likely missing a header, so from here we have to keep trying headers until we get a valid compressed file format.

Eventually, I tried the `zlib` format and found [this page](https://unix.stackexchange.com/questions/22834/how-to-uncompress-zlib-data-in-unix) that gives a one-liner to decompress a file with missing headers.

```sh
nicolas:/tmp/cybersky:$ printf "\x1f\x8b\x08\x00\x00\x00\x00\x00\x00\x00" |cat - file.raw |gzip -dc >/tmp/out
nicolas:/tmp/cybersky:$ file /tmp/out
/tmp/out: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, progressive, precision 8, 500x567, components 3
nicolas:/tmp/cybersky:$ xdg-open /tmp/out
```

![zlib](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/zlib.jpg)

And here we get the flag!

## Deleted Flag (Hard)
#### Description: "We have managed to secure a hacker's laptop but in the process of imaging the computer, it self destructed. We are not able to boot into the image, can you recover the information we need?"
#### Provided files: [usb.img.gz](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/post_assets/NCL_Practice/usb.img.gz)

For this one, I'm going to work through the challenges in a slightly different order than provided.

`Q5: How many hidden sectors are there on the disk?`

`Q6: What file system was used in this disk image?`

These two, I got pretty quick by running the `file` command on the image:

```sh
nicolas:/tmp/cybersky:$ file usb.img                                                   
usb.img: DOS/MBR boot sector, code offset 0x52+2, OEM-ID "NTFS    ", sectors/cluster 8, Media descriptor 0xf8, sectors/track 63, heads 255, hidden sectors 2201600, dos < 4.0 BootSector (0), FAT (1Y bit by descriptor); NTFS, sectors/track 63, physical drive 0x80, sectors 307199, $MFT start cluster 12800, $MFTMirror start cluster 2, bytes/RecordSegment 2^(-1*246), clusters/index block 1, serial number 050f62a22f62a08b4; contains bootstrap BOOTMGR
```

Here we can see that there are 2201600 hidden sectors, and that it's an NTFS file system.

### Q3: How many deleted user (non-root) directories are there?

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb1.png)

I've now imported the image in autopsy as a volume. After going to the file analysis page and checking deleted files, I've found two different user home directories.

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb2.png)

### Q8: What is the flag?

Checking the deleted flag.jpg file, we can see the flag written out in the image:

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb3.png)

### Q2: How many active user (non-root) directories are there?

After checking `/home/`, we can see that there are 6 active user directories.

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb4.png)

### Q4: Which user moved the flag onto the system?

Upon checking the `/root/` directory, we can see a history file:

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb5.png)

`mv /mnt/usb/flag.jpg /etc/hidden/`

this line shows that the `root` user moved the flag onto the system.

### Q1: What Operating System distribution was the hacker running?

In `/etc/os-release`, we can see the operating system and version:

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb6.png)
	
### Q7: What is the GUID of the volume?

Under `/`, we can see the `/System Volume Information` directory, in which we find the `IndexerVolumeGuid` file:

![Autopsy](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/usb7.png)

Looking at the hex data for the file, we can see the GUID separated by null bytes:
```hexdump
                        CONTENT

00000000:  7B00 4600 4500 4100 3700 3500 3000 3400    {.F.E.A.7.5.0.4.
00000010:  3200 2D00 3300 4300 3600 3500 2D00 3400    2.-.3.C.6.5.-.4.
00000020:  3800 3900 4300 2D00 4100 4500 3900 4600    8.9.C.-.A.E.9.F.
00000030:  2D00 3900 4100 3100 3500 3900 3500 3800    -.9.A.1.5.9.5.8.
00000040:  3600 4600 3300 4600 3300 7D00              6.F.3.F.3.}.
```

Removing these allows us to obtain the GUID:
`{FEA75042-3C65-489C-AE9F9A159586F3F3}`

# Final thoughts

Even though this was just the practice round, I had a lot of fun working my way through the challenges! I'm very happy with my noticable skill improvement, and I'm definitely excited to see how the individual and team rounds go.

Make sure to check out my other writeups!

![Scorecard](https://raw.githubusercontent.com/Glitch-Gecko/glitch-gecko.github.io/main/images/NCL_SP_2024/score.png)