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

![run_forest](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reversing-warmup_1.JPG)

----

# Reversing Warmup 2

Can you decode the following string dGg0dF93NHNfczFtcEwz from base64 format to ASCII?

<details>
  <summary>Hints</summary>
  
    1. Submit your answer in our competition's flag format.
</details>

## Solution

Converting it gives the flag:

![convertte](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reverseing-warmup_2.JPG)

----

# Assembly-0

What does asm0(0xaa,0xf2) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source located in the directory at /problems/assembly-0_2_485b2d48345b19addbeb06a36aabdc74.

<details>
  <summary>Hints</summary>
  
    1. basical <a href="https://www.tutorialspoint.com/assembly_programming/assembly_basic_syntax.htm">assembly</a> tutorial
    2. assembly <a href="https://www.tutorialspoint.com/assembly_programming/assembly_registers.htm">registers</a>
</details>

## Solution

Following the assembly and the arguments passed specified in the question:

```assembly_x86
.intel_syntax noprefix
.bits 32
	
.global asm0

asm0:
	push	ebp
	mov	ebp,esp
	mov	eax,DWORD PTR [ebp+0x8] ; first param (0xaa)
	mov	ebx,DWORD PTR [ebp+0xc] ; second param (0xf2)
	mov	eax,ebx ; eax has 0xf2
	mov	esp,ebp
	pop	ebp	
	ret
	
asm0(0xaa, 0xf2) = 0xf2
```

----

# Assembly-1

What does asm1(0x76) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source located in the directory at /problems/assembly-1_0_cfb59ef3b257335ee403035a6e42c2ed.

<details>
  <summary>Hints</summary>
  
    1. assembly <a href="https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm">conditions</a>
</details>

## Solution

Following the assembly and the arguments passed specified in the question:

```assembly_x86
.intel_syntax noprefix
.bits 32
	
.global asm1

asm1:
	push	ebp
	mov	ebp,esp
	cmp	DWORD PTR [ebp+0x8],0x98 ; 0x76 > 0x98
	jg 	part_a	; jmp not taken
	cmp	DWORD PTR [ebp+0x8],0x8 ; 0x76 != 0x8
	jne	part_b ; jump taken
	mov	eax,DWORD PTR [ebp+0x8]
	add	eax,0x3
	jmp	part_d
part_a:
	cmp	DWORD PTR [ebp+0x8],0x16
	jne	part_c
	mov	eax,DWORD PTR [ebp+0x8]
	sub	eax,0x3
	jmp	part_d
part_b:
	mov	eax,DWORD PTR [ebp+0x8] ; eax = 0x76
	sub	eax,0x3 ; eax = eax-3 (0x73)
	jmp	part_d ; unconditional jump
	cmp	DWORD PTR [ebp+0x8],0xbc
	jne	part_c
	mov	eax,DWORD PTR [ebp+0x8]
	sub	eax,0x3
	jmp	part_d
part_c:
	mov	eax,DWORD PTR [ebp+0x8]
	add	eax,0x3
part_d:
	pop	ebp
	ret

asm1(0x76) = 0x73
```

----

# Be Quick Or Be Dead

You find [this](https://www.youtube.com/watch?v=CTt1vk9nM9c) when searching for some music, which leads you to [be-quick-or-be-dead-1](https://github.com/bear-sec/pico2018/blob/master/Reverse%20Engineering/5%20-%20be-quick-or-be-dead/be-quick-or-be-dead-1?raw=true). Can you run it fast enough? You can also find the executable in /problems/be-quick-or-be-dead-1_0_0ba9c6f09fe8b3168d2743ddc4919008.

<details>
  <summary>Hints</summary>
  
    1. What will the key finally be?
</details>

## Solution

First thing is we check whats in main

![main](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reversing-quick1_1.JPG)

we see 4 consecutive calls to 4 funtions: header, set_timer, get_key, print_flag.
running the app just hangs, and reaches a timeout that was set presumably in set_timer so we will need to avoid that, or just fix the hanging.
lets see whats in header:

![header](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reversing-quick1_2.JPG)

just printing the title of the challenge.
set_timer sets the function alarm_handler to run after 1 second and terminate the program, we can skip this call by patching the program.
get_key is where the program hangs, so lets investimagate. (Investimagate, verb: to investigate, but with the added implication of shenanigans to be had)
inside the function theres a function call to calculate_key, maybe this is what hanging the program, lets see whats inside

![calculator](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reversing-quick1_3.JPG)

a loop while var_4 != 0xdfea967a, it adds one to it. *sigh*
we can set cs:key global var to 0xdfea967a and skip the call to it.
doing so gives us the flag.

----

# Quackme

Can you deal with the Duck Web? Get us the flag from [this program](https://github.com/bear-sec/pico2018/blob/master/Reverse%20Engineering/6%20-%20quackme/main?raw=true). You can also find the program in /problems/quackme_3_9a15a74731538ce2076cd6590cf9e6ca.

<details>
  <summary>Hints</summary>
  
    1. Objdump or something similar is probably a good place to start.
</details>

## Solution

In the main function we see a call to do_magic

![mian](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reversing-quack_1.PNG)

this function loops through our input whicis stored in s, and some hardcoded byte array, and xors each byte at a time. 

![quack](https://github.com/bear-sec/bear-sec.github.io/raw/master/images/reversing-quack_2.PNG)

presumably the correct input is these arrays xored. we can also see that the good boy message checks the validity of the string so lets write a script that solves this.

```python

a = 'You have now entered the Duck Web, and you'[:25]
b = [0x29, 0x6, 0x16, 0x4f, 0x2b, 0x35, 0x30, 0x1e, 0x51, 0x1b, 0x5b, 0x14, 0x4b, 0x8, 0x5d, 0x2b, 0x52, 0x17, 0x1, 0x57, 0x16, 0x11, 0x5c, 0x7, 0x5d]

ting = ''
for i in range(25):
        ting += chr(ord(a[i]) ^ b[i])
print ting
```

----

# Assembly-2

What does asm2(0x4,0x2d) return? Submit the flag as a hexadecimal value (starting with '0x'). NOTE: Your submission for this question will NOT be in the normal flag format. Source located in the directory at /problems/assembly-2_2_39150748a2771e0f5d2cbb14351ba582.

<details>
  <summary>Hints</summary>
  
    1. assembly <a href="https://www.tutorialspoint.com/assembly_programming/assembly_conditions.htm">conditions</a>
</details>

## Solution

We follow the program with the supplied arguments.
first the code stores the arguments in some local variables, then it unconditionally jumps to part_b.

```assembly
.intel_syntax noprefix
.bits 32
	
.global asm2

asm2:
	push   	ebp
	mov    	ebp,esp
	sub    	esp,0x10
	mov    	eax,DWORD PTR [ebp+0xc] ; eax = 0x2d
	mov 	DWORD PTR [ebp-0x4],eax ; some_local = 0x2d
	mov    	eax,DWORD PTR [ebp+0x8] ; eax = 0x4
	mov		DWORD PTR [ebp-0x8],eax ; some_local2 = 0x4
	jmp    	part_b
part_a:	
	add    	DWORD PTR [ebp-0x4],0x1 ; 0x2d + 1
	add		DWORD PTR [ebp+0x8],0x64 ; 0x4 + 0x64
part_b:	
	cmp    	DWORD PTR [ebp+0x8],0x1d89 ; 0x4 < 0x1d89
	jle    	part_a ; jmp taken
	mov    	eax,DWORD PTR [ebp-0x4]
	mov		esp,ebp
	pop		ebp
	ret

asm2(0x4, 0x2d)
```

prat_b checks if arg_1(0x4) is less than 7561 and if not it adds 100 to arg_1 and 1 to local1 which it returns.
i have transfered this assembly code to python and ran it, the result is the flag.

```python
def asm2(int1, int2):
    local1 = int2
    local2 = int1
    while int1 < 0x1d89:
        local1 += 1
        int1 += 0x64
    return local1

print hex(asm2(0x4, 0x2d))
```

----

