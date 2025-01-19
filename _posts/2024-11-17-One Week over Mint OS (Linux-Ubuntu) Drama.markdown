---
layout: post
title:  One Week over Mint OS (Linux-Ubuntu) Drama
date:   2024-11-17 02:00:14 +0930
categories: linux experience
---
# Background
Several weeks ago, I bought a new PC with a dual operating system - Windows 11 and Mint OS installed. Last weekend, I finally got some free time to modify brand-new Mint OS. But this is what the whole drama really started. 

Linux (and Ubuntu) weren't new to me. I used them when I was a research student in the university - installing Ubuntu and Tails OS systems on a Raspberry Pi module or a USB stick, using some Linux command lines on Windows Subsystem for Linux (WSL) for fun. Before that, I have also been using Mac OS system (a UNIX-based system) for approximately six years (also using Wine containers or Parallels Desktop Virtual Machine for accessing Windows based software for my studies, such as APSIM). However, this time was my _FIRST_ time really seriously using Linux. 

To be fair and honest, I am not good at coding. Coding for me has been a steep learning curve for last four years.  Before downloading and installing Linux permanently, I have already heard from people that coding is essential to use Linux, and this is probably because Linux is easily broken - many of basic uses involve coding and troubleshooting, such as installing the software by `sudo apt-get install`. 


**Challenges accepted!**


# Issue 1 - Step into a "trap"
While I was using the Mint OS, my cursor couldn't move around. Luckily, the keyboard was working, and I did not feel panic about it, because I mentally prepared for anything going wrong. However, this time I didn't realise I went too far! 

I forced the PC to shut down, and attempted to restart in the hope of resolving the issue automatically picked up by the system. Unfortunately, it didn't, so I was looking for the possible solution online. 

_Many seconds, hours, days have gone through_ **Oops, I messed up the boot**

![I did many starts]({{ site.baseurl }}/assets/media/2024-11-17/2024-11-11-234606_002.jpeg "BIOS display")

# Issue 2 - Can't boot up the Mint OS
It was bit late that I couldn't boot up Mint OS. At the beginning, I was bit annoyed but I was optimistic that I could fix it up. Over the course of several nights, while I was searching the possible solutions, I started losing patience, and felt upset that I wouldn't be able to use Linux at all in near future. Then I was thinking it might be a good idea to reinstall another Linux-based OS, such as Arch OS or Debian OS. I gave it up after I was attempting to install them. It was a very _complicated_ process. <br>
![Installation for Debian OS]({{ site.baseurl }}/assets/media/2024-11-17/2024-11-12-132835_002.jpeg "Debian error")
![Installation for Arch OS]({{ site.baseurl }}/assets/media/2024-11-17/2024-11-12-212255_002.jpeg "No idea...")

# Pivotal success
While I was talking my friend, I learnt that there is a boot repair tool from [Ubuntu](https://help.ubuntu.com/community/Boot-Repair){:target="_blank"} might be able to help me from this situation. I wipe the only USB out and to boot the live session from this boot repair tool. I followed the instruction, and it did not help. I have to seek the help from other people on [Ubuntu Forum](https://ubuntuforums.org/showthread.php?t=2502458){:target="_blank"}.

On the second day, I discovered the boot was deleted from the booting list. The solution code as follows: <br>
`root@ubuntu:~# efibootmgr -c -w -L "Mint" -d /dev/nvme0n1p5 -p 1 -l /efi/ubuntu/grubx64.efi `

# Distorted display resolution 
Cheerful moment didn't last for a long time. 
![Scale can't be modified]({{ site.baseurl }}/assets/media/2024-11-17/2024-11-16-182422_002.jpeg "too large!!")

I found you can actually set up a new mode for this display by following these codes:

`cvt 1920 1200` # it will give the output for _Modeline "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync_ 
<br>
`xrandr --newmode  "1920x1080_60.00"  173.00  1920 2048 2248 2576  1080 1083 1088 1120 -hsync +vsync`
<br>
`xrandr --addmode None-1 "1920x1080_60.00"`

But this gave me a **black** screen! At least, I can still boot up, so I tried the last resort - Recovery mode. 

_Eventually, successfully done!_
