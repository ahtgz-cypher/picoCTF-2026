# 🚩 picoCTF 2026 Writeups

> My journey solving picoCTF challenges 🧠💻

---

## 📌 About
- 👤 Author: ahtgz
- 🎯 Goal: Learn cybersecurity & CTF
- 🏆 Platform: picoCTF

---

## 📂 Challenge
```
Timeline 1
Author: LT 'syreal' Jones

Description
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format.
Download the disk image here. https://challenge-files.picoctf.net/c_plain_mesa/fef9e3937fced503da228c6affaea69ed51d6234ed8fde14a52b573777b869e7/partition4.img.gz
Hints 
Create a Sleuthkit MAC timeline!
Look at recent timestamps
Pay close attention to timestamps near an anti-forensic action
Filter only new files by grepping for macb
```


#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
First, I wget and gunzip file, Read the first hint: Create a Sleuthkit MAC timeline!
I create file body.txt contain all metadata of file img (inode, path, time MACB)
```
fls -r -m / partition4.img > body.txt
mactime -b body.txt > timeline.txt
-r: recursive ( scan entire file )
-m: mount point file, attach link
and mactime: covert body.txt -> timeline (easy to read over time)
```
MACB mean:
M = modified (file content changed) 
A = Accessed (file be read)
C = Changed (metadata changed)
B = Birth (file be create)
Why we should do it?
Because in forensics: We don't know what file contain flag 
But we know when attacker action
And after we have timeline, we will:
```
tail -n 50 timeline.txt
```
And we will see:
shred
poweroff 
new file /etc/chat
We don't need find file one by one
--> We do it to turn disk image into timeline to see what attacker do before he erase the traces
I noticed that: 
/usr/bin/shred appear
poweroff
/root/.ash_history changed
/root be access
Insight: Behavior erase the traces (anti-forensics) 
Next, defind time attacker attack about:
Dec 01 ~ 16:50
This is the most important timeline 
We will find file be create near that time, we use hint: Filter only new files by grepping for macb
```
mactime -b body.txt | grep macb
```
And we see: /etc/chat (inode 32716)
Why it suspicious:
It not standart system file
It apppear in same time attack 
It in /etc (weird location)
Next, we dump it:
```
icat partition4.img 32717 > chat
```
Analyze it: strings chat 
And see base64, decode it and take flag.
