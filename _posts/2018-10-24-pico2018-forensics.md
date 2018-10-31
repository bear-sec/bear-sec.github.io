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

We captured [some traffic](https://github.com/bear-sec/pico2018/blob/master/Forensics/6%20-%20admin%20panel/data.pcap?raw=true) logging into the admin panel, can you find the password?
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

# Hex Editor

[This cat](https://github.com/bear-sec/pico2018/raw/master/Forensics/7%20-%20hex%20editor/hex_editor.jpg) has a secret to teach you. You can also find the file in /problems/hex-editor_4_0a7282b29fa47d68c3e2917a5a0d726b on the shell server.

<details>
  <summary>Hints</summary>
  
    1. What is a hex editor? Maybe google knows.
        <a href="https://linux.die.net/man/1/xxd">xxd</a><br>
        <a href="https://linux.die.net/man/1/hexedit">hexedit</a><br>
        <a href="https://linux.die.net/man/1/bvi>bvi</a><br>
</details>

## Solution

In this challenge we get another image file and we need to find the secret. since the challenges name is hex-editor, i opened the image file in my favorite hex editod, and searched for pico, and found the flag in the raw bytes.

![pico](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-hex_1.PNG)

----

# Truly and Artist

Can you help us find the flag in this [Meta-Material](https://github.com/bear-sec/pico2018/raw/master/Forensics/8%20-%20Truly%20an%20Artist/2018.png)? You can also find the file in /problems/truly-an-artist_2_61a3ed7216130ab1c2b2872eeda81348.

<details>
  <summary>Hints</summary>
  
    1. Try looking beyond the image. <br>
    2. Who created this?
</details>

## Solution

Another image in this category, this time the flag is in the meta-material, and for images its exif data. Any exif tool will do, ive just threw it in an online one:

![arteest](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-artist_1.JPG)

----

# Now You Dont

We heard that there is something hidden in [this picture](https://github.com/bear-sec/pico2018/raw/master/Forensics/9%20-%20now%20you%20dont/nowYouDont.png). Can you find it?

<details>
  <summary>Hints</summary>
  
    1. There is an old saying: if you want to hide the treasure, put it in plain sight. Then no one will see it.<br>
    2. Is it really all one shade of red?
</details>

## Solution

From the description and hints we can undestand that we must search for some hidden image, and again with stegsolve, since this will be in the red plane, we search for it in that plane. we see right away that its in the 1 and 0 bit plane. 

![reds](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-dont_2.PNG)

side note, we could solve this by some random color map that inverts some shades to others randomly, and see it, as shown here.

![randoms](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-dont_1.PNG)

----

# Ext Super Magic

We salvaged a ruined [Ext SuperMagic II-class](https://github.com/bear-sec/pico2018/raw/master/Forensics/10%20-%20Ext%20Super%20Magic/ext-super-magic.img) mech recently and pulled the filesystem out of the black box. It looks a bit corrupted, but maybe there's something interesting in there. You can also find it in /problems/ext-super-magic_1_c544657e659accef770d3f2bc8384a8c on the shell server.

<details>
  <summary>Hints</summary>
  
    1. Are there any <a href="https://en.wikipedia.org/wiki/Fsck">tools</a> for diagnosing corrupted filesystems? What do they say if you run them on this one?<br>
    2. How does a linux machine know what <a href="https://www.garykessler.net/library/file_sigs.html" >type</a> of file a <a href="https://linux.die.net/man/1/file">file</a> is? <br>
    3. You might find this <a href="http://www.nongnu.org/ext2-doc/ext2.html">doc</a> helpful.<br>
    4. Be careful with <a href="https://en.wikipedia.org/wiki/Endianness">endianness</a> when making edits.<br>
    5. Once you've fixed the corruption, you can use /sbin/<a href="https://linux.die.net/man/8/debugfs">debugfs</a> to pull the flag file out.<br>
</details>

## Solution

In this challenge we receive a filesystem that is as stated corrupt. First thing is to backup the image file, since they warn us to be careful, we will be responsible and backup the image file.
Lets check whats corrupt, so we try fsck(or e2fsck for an image file):

![corruption](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-ext_1.JPG)

We see that the magic header is corrupt, so we find where it should be and what it should be

![should_be](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-ext_2.JPG)

Editing the magic value "0xEF53" where it supposed to be with HXD, now we can use debugfs as said in the challenge to browse through the FS.

![deebug](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-ext_3.JPG)

Now we can just dump the flag.

![dump](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-ext_4.JPG)

flag:

![flag:](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-ext_5.jpg)

----

# Lying Out

Some [odd traffic](https://github.com/bear-sec/pico2018/raw/master/Forensics/11%20-%20Lying%20Out/traffic.png) has been detected on the network, can you identify it? More info here. Connect with nc 2018shell1.picoctf.com 27108 to help us answer some questions.

No hints for this one.

## Solution

We get a picture of some traffic graph that we need to answer some question based upon. as we connect to the port, we receive some questions based on the data supplied, answering them correctly gets the flag. 

![question](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-lying_1.PNG)

----

# Whats My Name

Say my name, [say my name](https://github.com/bear-sec/pico2018/blob/master/Forensics/12%20%20-%20Whats%20My%20Name/myname.pcap?raw=true).

<details>
  <summary>Hints</summary>
  
    1. If you visited a website at an IP address, how does it know the name of the domain?
</details>

## Solution

We get a pcap and we need to "say its name". In the hints, we see that we need to translate an IP. so first thing i tried is to extract all of the ips:

![tshark](https://github.com/bear-sec/bear-sec.github.io/blob/master/images/forensics-myname_1.PNG)

Now run a dns query for each one, done with python, to see maybe the dns name is the flag. 

```python
import socket

ips = open(r'C:\Users\Gera\Documents\GitHub\pico2018\Forensics\12  - Whats My Name\ips2.txt',
           'r').readlines()
for ip in ips:
    try:
        print socket.gethostbyaddr(ip.strip())
    except:
        print ip.strip(), 'not found'

```

this didt reveal any obvious solution:

![not_obvious](https://github.com/bear-sec/bear-sec.github.io/blob/master/images/forensics-myname_2.PNG)

I figured I need to check the pcap by hand, and as easy as that, filtering by DNS, revealed the answer. 

![by_hand](https://github.com/bear-sec/bear-sec.github.io/blob/master/images/forensics-myname_3.PNG)

---

# Core

[This program](https://github.com/bear-sec/pico2018/blob/master/Forensics/13%20-%20core/print_flag?raw=true) was about to print the flag when it died. Maybe the flag is still in [this core](https://github.com/bear-sec/pico2018/blob/master/Forensics/13%20-%20core/core?raw=true) file that it dumped? Also available at /problems/core_0_28700fe29cea151d6a3350f244f342b2 on the shell server.

<details>
  <summary>Hints</summary>
  
    1. What is a core file?
    2. You may find <a href="https://darkdust.net/files/GDB%20Cheat%20Sheet.pdf">this</a> reference helpful.
    3. Try to figure out where the flag was read into memory using the disassembly and <a href="https://linux.die.net/man/1/strace">strace</a>.
    4. You should study the format options on the cheat sheet and use the examine (x) or print (p) commands. disas may also be useful.
</details>

## Solution

we get a program and its core dumped as it says "...was about to print the flag..." so we know we need to somehow analyze the program and the coredump. we will use gdb for that. 
First thing we need to see how to work with coredumps in gdb.

![cores](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-core_1.PNG)

So we dump the files into our linux machine and begin the analysis.
first we check where the program crashed. we see thats its at address 0x80487c1, which is beginnig of print_flag function.

![beginning](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-core_2.PNG)

so we disassemble the function and see what it does.

![it_does](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-core_3.1.png)

We see that it loads an addresds into eax, and passes it to prints as second parameter, and the formatted string is "`"your flag is: picoCTF{% raw %}{%s}{% endraw %}\n"`", so the address is the flag. We resolve it and get that its at offset 0x14e4 from 0x804a080. which is an address, and not a string. we dereference it and print, and get the flag.

![dereference](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-core_3.2.png)

----

# Malware Shops

There has been some [malware detected](https://github.com/bear-sec/pico2018/raw/master/Forensics/14%20-%20Malware%20Shops/plot.png), can you help with the analysis? [More info](https://github.com/bear-sec/pico2018/raw/master/Forensics/14%20-%20Malware%20Shops/info.txt) here. Connect with nc 2018shell1.picoctf.com 46168.

No hints for this one.

## Solution

we get a plotted data set that resembles the data set we got in "lying out" and an info file that basically states that people that write malware use the same amount of jumps and adds in their code, across different samples from the same actor. so we connect to the port supplied and answer some questions based on the data from the graph and get the flag.

This is the second of two similar challenges that used basically the same graph pattern. These two challenges werent interesting in any way, and required knowing english well, rather than analyzing malware. Thumbs down for these two.. But oh well, points are point.

----

# LoadSomeBits

Can you find the flag encoded inside [this image](https://github.com/bear-sec/pico2018/raw/master/Forensics/15%20-%20LoadSomeBits/pico2018-special-logo.bmp)? You can also find the file in /problems/loadsomebits_3_8933ebe9085168b1e0bbb07884c2231f on the shell server.

<details>
  <summary>Hints</summary>
  
    1. Look through the Least Significant Bits for the image
    2. If you interpret a binary sequence (seq) as ascii and then try interpreting the same binary sequence from an offset of 1 (seq[1:]) as ascii do you get something similar or completely different?
</details>

## Solution

in this challenge we get an image with some data hidden in it. as the hints say, its LSB steganography and they said not to just extract and represend as ascii data, rather check if it checks out if we look at the bit sequence from index 1.
Lets try writing some python code to solve it.

```python
import struct
import binascii

a = open(r'C:\Users\Gera\Documents\GitHub\pico2018\Forensics\15 - LoadSomeBits\pico2018-special-logo.bmp',
         'rb').read()
data_offset = struct.unpack("<I",a[0xa:0xe])[0] # bitmap offset
print "bmp data at: ", hex(data_offset)
for i in range(8): # possible offsets
    data = a[data_offset+i:data_offset+0x200+i]
    bits = [ord(x)&1 for x in data]
    dwords = [''.join([str(y) for y in bits[x*8:x*8+8]]) for x in range(len(bits)/8)]
    flag = ''.join([chr(int(x,2)) for x in dwords])
    print "possible flag: " + flag + " at offset: " + str(i)
```

we see that we get the flag at offset 4, but were missing some data.

![miss](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-lsb_1.png)

Back to the image file in a hex editor, we see that the data starts at offset 0x37, so we set this as the start.
and maybe sooner, so lets set 0x30.

```python
data_offset = 0x30
```

and voila, we get the flag

![](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/forensics-lsb_2.png)

----

