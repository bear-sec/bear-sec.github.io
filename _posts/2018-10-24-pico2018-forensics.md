---
layout: post
title: PicoCTF 2018 - Forensics
---

This post will be focusing on the "Forensics" challenges presented in picoCTF. These challenges were supposed to be about forensics analysis, which there were not many. <br>
All the resources for these challenges, including most of the files, you can find in [my repository](https://github.com/bear-sec/pico2018 "picoCTF2018 writeups").

# Forensics Warmup 1

Can you unzip this file for me and retreive the flag?

<details>
  <summary>Hints</summary>
  
    1. Make sure to submit the flag as {% raw %}picoCTF{XXXXX}{% endraw %} as the flag.
</details>

## Solution

The supplied file is a zip archive, and by just opening the archive we find a picture that has the flag, we can submit it.

![zeep](https://bear-sec.github.io/images/forensics-warmup_1.JPG)

----

# Forensics Warmup 2

Hmm for some reason I can't open this PNG? Any ideas?

<details>
  <summary>Hints</summary>
  
    1. How do operating systems know what kind of file it is? (It's not just the ending!)
    2. Make sure to submit the flag as {% raw %}picoCTF{XXXXX}{% endraw %}
</details>

## Solution

We get a png file that as the challenge says we cant open, I guess it wasnt supposed to have an extension and we needed to check the file type, but it was a .png file.

![pengee](https://bear-sec.github.io/images/forensics-warmup_2.png)

----

# Desrouleaux

Our network administrator is having some trouble handling the tickets for all of of our [incidents](https://github.com/bear-sec/pico2018/raw/master/Forensics/3%20-%20Desrouleaux/incidents.json). Can you help him out by answering all the questions? Connect with nc 2018shell1.picoctf.com 10493.

<details>
  <summary>Hints</summary>
  
    1. If you need to code, python has some good libraries for it.
</details>

## Solution

As we connect to the supplied port, we get a prompt for some questions, each question is based on the json file, mind that each time the addresses are different.
```python
from collections import Counter
import json


a = json.load(open(r'C:\Users\Gera\Downloads\incidents.json','r'))
b = a['tickets']
print 'chal 1'
print Counter([x['src_ip'] for x in b]).values()
print Counter([x['src_ip'] for x in b]).keys()
print 'chal 2'
print [x['dst_ip'] if x['src_ip']=='21.201.106.115' else None for x in b]
print 'chal 3'
print Counter([x['file_hash'] for x in b]).values()
print Counter([x['file_hash'] for x in b]).keys()
```

----

# Reading Between The Eyes

Stego-Saurus hid a message for you in [this image](https://github.com/bear-sec/pico2018/raw/master/Forensics/4%20-%20Reading%20Between%20the%20Eyes/husky.png), can you retreive it?

<details>
  <summary>Hints</summary>
  
    1. Maybe you can find an online decoder?
</details>

## Solution

in this challenge we get an image and not much data besides that. its said that theres a message hidden in the image, so the first thing to do is try and see if we can easily see something odd in the image layers. Ive chose to use my favorite tool for steganography, which is stegsolve.
we can right away see that in each color, the 0 bit plane has some data in the beginning and then its 0.

![green](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-reading_1.PNG)
![blue](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-reading_2.PNG)

We can also see that plane 1 is proper static noise (this happens at all colors)

![noise](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-reading_3.PNG)

Now we extract 0 plane from all the planes, and get the ascii string of the flag:

![flag](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-reading_4.PNG)

----

# Recovering From the Snap

There used to be a bunch of animals [here](https://github.com/bear-sec/pico2018/blob/master/Forensics/5%20-%20Recovering%20From%20the%20Snap/animals.dd?raw=true), what did Dr. Xernon do to them?

<details>
  <summary>Hints</summary>
  
    1. Some files have been deleted from the disk image, but are they really gone?
</details>

## Solutions

We are presented with a .dd file. The challenge also says that there are a bunch of animals, maybe pictures? so we use foremost to carve out image file types. 

![scalp](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-recover_1.JPG)

and we find a folder of jpg's with some animals, and in this folder a picture of a flag.

![scalp](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-recover_2.JPG)

----

# Admin Panel

We captured some traffic logging into the admin panel, can you find the password?
<details>
  <summary>Hints</summary>
  
    1. Tools like wireshark are pretty good for analyzing pcap files.
</details>

## Solution

Inside the data pcap file, we see http traffic. we see that the first login is for a regular user:

![noob user](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-admin_1.JPG)

and then theres another login with the admin user, and the password is the format of the flags:

![cool user](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-admin_2.JPG)

----

