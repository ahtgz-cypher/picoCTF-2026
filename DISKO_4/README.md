# 🚩 picoCTF 2026 Writeups

> My journey solving picoCTF challenges 🧠💻

---

## 📌 About
- 👤 Author: ahtgz
- 🎯 Goal: Learn cybersecurity & CTF
- 🏆 Platform: picoCTF

---

## 📂 Challenges
- DISKO 4
- Author: Darkraicg492

- Description
Can you find the flag in this disk image? This time I deleted the file! Let see you get it now!
Download the disk image here.https://challenge-files.picoctf.net/c_plain_mesa/7ccb9163cdca38fb1317009dbfc22c0e268ffc4fe4deedd13fb4ffab2bd30731/disko-4.dd.gz
- Hints 
How would you look for deleted files?


#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
First, I gunzip file .gz and get file disko-4.dd, try check file by command:
```
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Disko4]
└─$ file disko-4.dd            
disko-4.dd: DOS/MBR boot sector, code offset 0x58+2, OEM-ID "mkfs.fat", Media descriptor 0xf8, sectors/track 32, heads 8, sectors 204800 (volumes > 32 MB), FAT (32 bit), sectors/FAT 1576, serial number 0x49838d0b, unlabeled
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Disko4]
└─$ mmls disko-4.dd

```
