# 🚩 picoCTF 2026 Writeups

> My journey solving picoCTF challenges 🧠💻

---

## 📌 About
- 👤 Author: ahtgz
- 🎯 Goal: Learn cybersecurity & CTF
- 🏆 Platform: picoCTF

---

## 📂 Challenges
```
Forensics Git 1
Author: LT 'syreal' Jones

Description
Can you find the flag in this disk image?
Download the disk image here.https://challenge-files.picoctf.net/c_plain_mesa/4538dd1f2e93e907c17f0b663c0e1fae2d7054a72b4ee36977f20cfbf3b0a01c/disk.img.gz
Hints 
How can you checkout the files of a previous commit?
```

#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
I read Hint: How can you checkout the files of a previous commit?
It means flag in old commit of Git. This disk image contain repo git, but current commit not contain flag. So, the fastest way is extract entire filesystem and use git 
First, I use Sleuthkit to list filesystem .
```
fls -o 1140736 disk.img
```
-o: offset of partition
fls: list file/directory 
We see folder contain repo: d/d 65664: repo
We will go to repo:
```
fls -o 1140736 disk.img 65664
```
And I see:
```
d/d 65665: .git
```
We see construct Git repository: 
```
fls -o 1140736 disk.img 65665
```
And we see:
```
objects
refs
HEAD
config
index
```
This is the standard structure of Git. Git object will in .git/objects, we list:
```
fls -o 1140736 disk.img 65689
```
And I see git object hash: f1/50f47a5dabfb4397706aa18905df936595a86e
--> folder: 2 char of the first strings
--> file: the remaining 38 char 
And I dump git objects:
```
icat -o 1140736 disk.img 65695 > obj1
icat -o 1140736 disk.img 65698 > obj2
icat -o 1140736 disk.img 65700 > obj3
icat -o 1140736 disk.img 65708 > obj4
icat -o 1140736 disk.img 65709 > obj5
```
icat = extract raw file from disk image 
Git object compressed by zlib: 
Check hex: xxd obj1 | head
And see: 78 01
--> head of zlib compression
Decompress git object:
```
python3 -c "import zlib;print(zlib.decompress(open('obj1','rb').read()))"
```
And output:
```
blob 31
picoCTF{g17_r3m3mb3r5_d4ddf904}
```
blob = git object contain file content
31 = size 
and behind is content of file 
```
8️⃣ Vì sao hint nói "previous commit"

Hint:

checkout the files of a previous commit

Trong repo này:

commit mới đã xoá flag
nhưng git object vẫn còn trong .git/objects

Git không xoá object ngay, nên forensic vẫn đọc được.

9️⃣ Kiến thức Git forensic quan trọng

Git repo gồm 3 loại object:

blob

nội dung file

blob
hello.txt
tree

cấu trúc folder

tree
file1
file2
commit

metadata

commit
author
tree
parent
message

Tất cả đều nằm trong:

.git/objects

và đều zlib compressed.

🔟 Pattern của các bài Git forensic CTF

Flag thường nằm:

1️⃣ trong blob của commit cũ
2️⃣ trong .git/logs
3️⃣ trong deleted git object
4️⃣ trong pack files

💡 Tip cực nhanh cho các bài giống vậy

Nhiều khi chỉ cần:

strings disk.img | grep picoCTF

hoặc:

fls -r disk.img
```
