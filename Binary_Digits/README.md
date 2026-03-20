# 🚩 picoCTF 2026 Writeups

> My journey solving picoCTF challenges 🧠💻

---

## 📌 About
- 👤 Author: ahtgz
- 🎯 Goal: Learn cybersecurity & CTF
- 🏆 Platform: picoCTF

---

## 📂 Challenges

- Binary Digits
- Author: Yahaya Meddy

- Description
This file doesn't look like much... just a bunch of 1s and 0s. But maybe it's not just random noise. Can you recover anything meaningful from this?
Download the file here. https://challenge-files.picoctf.net/c_plain_mesa/5da9b14e68244128d275e45ba304b51dbd45554b4eb25ef68c74b4a7579c6626/digits.bin

```

### Forensics

#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
First, I wget file challenge, gunzip file partition4.img.gz and get file digits.bin, file just contain many bit 0,1 and i try convert it into other data types, i look and realize 32 bits on the begin of the file
``` 
11111111 11011000 11111111 11100000
```
Convert it into hex is: FF D8 FF E0. It is magic header of JPEG. I use python
```
data = open("digits.bin").read().strip()

out = bytearray()

for i in range(0, len(data), 8):
    byte = data[i:i+8]
    out.append(int(byte, 2))

open("out.jpg", "wb").write(out)
```
And run python3 solve.py and i obtained an image containing the flag 

<img width="800" height="500" alt="image" src="https://github.com/user-attachments/assets/745c7d75-9a5e-43c0-a3f5-ddcab0a76b63"
