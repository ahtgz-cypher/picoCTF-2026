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
Timeline 0
Author: LT 'syreal' Jones

Description
Can you find the flag in this disk image? Wrap what you find in the picoCTF flag format.
Download the disk image here. https://challenge-files.picoctf.net/c_plain_mesa/aa1f8ba93409887e081435732d7037c45b30a8442853bf07c9e84fe4d0e0bc19/partition4.img.gz
Hints 
Create a Sleuthkit MAC timeline!
Sloppy timestomping can yield strange (very old) timestamps
```


#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
First, I gunzip file .gz and get file partition4.img. After read hint, I will use timeline forensics of The Sleuth Kit to find file has timestamp abnormal. In linux filesystem ext4, each file has 3 time: MAC = Modified, Accessed, Changed
. This challenge counterfeit timestamp. This will create extremely accient times, such as: 1970, 1969, 1900. It most likely contains a flag. First, we creat bodyfile:
```
fls -r -m / partition4.img > bodyfile.txt
-r → recursive
-m / → mount point root
```
Creat timeline:
```
mactime -b bodyfile.txt > timeline.txt
```
mactime also belong The Sleuth Kit
Find timestamp abnormal:
```
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Timeline0] └─$ grep -v "/lib/modules" timeline.txt | head Tue Jan 01 1985 12:00:00 41 macb r/rrw-r--r-- 0 0 4945 /bin/bcab Mon Oct 18 2021 13:54:17 451 ma.. r/rrw-r--r-- 0 0 64994 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-4a6a0840.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 64995 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-5243ef4b.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 64996 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-524d27bb.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 64997 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-5261cecb.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 64998 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-58199dcc.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 64999 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-58cbb476.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 65000 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-58e4f17d.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 65001 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-5e69ca50.rsa.pub 451 ma.. r/rrw-r--r-- 0 0 65002 /usr/share/apk/keys/alpine-devel@lists.alpinelinux.org-60ac2099.rsa.pub
```
👉 1985 is an extremely unusual timestamp in a modern Linux system.
This is precisely a sign of timestomping, as hinted at in the picoCTF article.
```
/bin/bcab
inode = 4945
icat partition4.img 4945 > bcab
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Timeline0] └─$ cat bcab NzFtMzExbjNfMHU3MTEzcl9oM3JfNDNhMmU3YWYK
 decode base64 71m311n3_0u7113r_h3r_43a2e7af
```
And flag picoCTF{71m311n3_0u7113r_h3r_43a2e7af} 


