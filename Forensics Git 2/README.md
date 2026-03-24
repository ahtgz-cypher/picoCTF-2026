Forensics Git 2
Author: LT 'syreal' Jones

Description
The agents interrupted the perpetrator's disk deletion routine. Can you recover this git repo?
Download the disk image here. https://challenge-files.picoctf.net/c_plain_mesa/5815a97d35f9214aebb870586256eb241b51eddc4967fd7f3ca815a190012d54/disk.img.gz
Hints 
We think the deletion was interrupted before any git objects were touched
Idea for this article:
We have Hint: “deletion was interrupted before any git objects were touched”
So, git directory can be deleted/broken, but git objects still intact
We just need read git object -> don't need rebuild repo 

First, we extract filesystem from disk image
```
binwalk -e disk.img
```
And we have ext-root-0/
Next, we find repo: 
```
cd repo
find . -name ".git"
```
And we see: home/ctf-player/Code/killer-chat-app/.git
```
ls .git
objects/  logs/  HEAD ...
```
But git CLI fail:
```
git reset --hard
```
Can't run, because repo be broken metadata
And we try read object direcly, we list object: 
```
find .git/objects -type f
```
Decompress git object (most important)
```
for f in $(find .git/objects -type f); do
    zlib-flate -uncompress < "$f" 2>/dev/null | strings
done
```
Git object in essence is:
```
zlib("blog <size>\0<data>")
```
So, we have to decompress and we can read content of file 
```
┌──(kali㉿kali)-[~/…/home/ctf-player/Code/killer-chat-app]
└─$ for f in $(find .git/objects -type f); do
    zlib-flate -uncompress < "$f" 2>/dev/null | strings
done
tree 68
100755 client
100755 server
tree 99
100755 client
40000 logs
100755 server
commit 244
tree 6bf83de540f7d12cc3b683a83d69432e03d84509
parent e80b38b3322a5ba32ac07076ef5eeb4a59449875
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000
Remove secret hideout log
tree 99
100644 1.txt
100644 2.txt
.100644 3.txt
qxdD3
tree 99
100644 1.txt
100644 2.txt
.100644 4.txt
f'8w
tree 66
100644 1.txt
100644 2.txt
commit 239
tree 6bf83de540f7d12cc3b683a83d69432e03d84509
parent 26b809e0c41d8421f1126ed3a4eb06ad66e6d90a
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000
Add TV show chat log
commit 238
tree c931ae0868411e5f23656a2436e78a4c4699e18c
parent 2151ef0ccc15aed1ab88e1afdc7484aaeff211c4
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000
Add random chat log
blob 142
Eva: Are you caught up on The Last of Us?
Nina: Episode 5 wrecked me; the clickers were intense.
Eva: Can't wait for the next season already.
tree 99
100755 client
40000 logs
100755 server
blob 140
Pip: My cat thinks the keyboard is a napping pad now.
Lou: Maybe it's writing a novel in secret.
Pip: I'd read anything titled "Meowmoirs".
blob 25
#!/bin/bash
nc "$1" 9000
blob 134
Jade: Finally beat the Elden Ring DLC tonight.
Marco: Nice! That Messmer fight was brutal but so good.
Jade: Totally worth the grind.
commit 246
tree ead27e2bd5a0fc22868ffb629a768f82dfcda11c
parent 5827632e046a80a1e0d7b4fc5c7800dd539baeaf
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000
Add secret hideout chat log
commit 242
tree 201c707b43219a63c1d3499b29c7d539af079861
parent 2c0a9b2b15dce92f800393d5030c7454efc278ae
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000
Add video game chat log
tree 33
100644 1.txt
tree 99
100755 client
40000 logs
100755 server
commit 189
tree 5eb896e3ccd51175f66480cdb247fc45f3e8ac2d
author ctf-player <ctf-player@example.com> 1763549240 +0000
committer ctf-player <ctf-player@example.com> 1763549240 +0000
Add netcat scripts
tree 99
100755 client
40000 logs
w100755 server
blob 25
#!/bin/bash
nc -lvp 9000
blob 188
Rex: Meet at the old arcade basement for the secret hideout.
Jay: Ask Rusty at the door and use password picoCTF{g17_r35cu3_16ac6bf3}.
Rex: Bring the decoder map so we can plan the route.
```

