---
layout: post
title: PicoCTF 2018 - Reversing
---

This post will be focusing on the "Reversing" challenges presented in picoCTF. These challenges are mainly all sorts of crack-me's or just understanding the program in assembly.
I particularly love these sorts of challenges, since they give me a chance to get some hands-on reversing experience, which is a skill i like.
All the resources for these challenges, including most of the files, you can find in [my repository](https://github.com/bear-sec/pico2018 "picoCTF2018 writeups").

# Reversing Warmup 1

Throughout your journey you will have to run many programs. Can you navigate to /problems/reversing-warmup-1_0_f99f89de33522c93964bdec49fb2b838 on the shell server and run [this program](https://github.com/bear-sec/pico2018/blob/master/Reverse%20Engineering/1%20-%20Reversing%20Warmup%201/run?raw=true) to retreive the flag?

<details>
  <summary>Hints</summary>
  
    1. If you are searching online, it might be worth finding how to exeucte a program in command line.
</details>

## Solution

Just running the program gives out the flag.

![run_forest](https://github.com/bear-sec/bear-sec.github.io/blob/master/images/reversing-warmup_1.JPG)

----

# Reversing Warmup 2

Can you decode the following string dGg0dF93NHNfczFtcEwz from base64 format to ASCII?

<details>
  <summary>Hints</summary>
  
    1. Submit your answer in our competition's flag format.
</details>

## Solution

Converting it gives the flag:

![convertte](https://github.com/bear-sec/bear-sec.github.io/blob/master/images/reversing-warmup_2.JPG)

----

