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
Forensics Git 0
Author: LT 'syreal' Jones

Description
Can you find the flag in this disk image?
Download the disk image here. https://challenge-files.picoctf.net/c_plain_mesa/96db2eea3d6d3e215d3dc2289457a1bc10b17b1de69c46996a171f4f689db74b/disk.img.gz
Hints 
How can you extract the directory from the disk image?
```

#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
First, I gunzip file .gz and get file disko-4.dd, try check file by command:
Trong các bài của picoCTF, flag thường nằm trong filesystem Linux, nên ta phải mount hoặc browse partition đó. 
Bước 1 — Dùng offset để xem filesystem
```
mmls disk.img
fls -r -o 2048 disk.img
fls -r -o 1140736 disk.img
```
Tìm những path đáng nghi như:
```
/home
/root
/tmp
/opt
```
Bước 3 — Extract directory hoặc file
```
icat -o 2048 disk.img 1234 > flag.txt
```
```
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Forensics Git 0]
└─$ fls -r -o 1140736 disk.img | grep ".git"
++++ d/d 65665: .git
++++++++ r/r 1741:      rc-digittrade.ko.gz
++++++++ r/r 1783:      rc-medion-x10-digitainer.ko.gz
++++++++ r/r 1740:      rc-digitalnow-tinytwin.ko.gz
++++++++ r/r 1929:      dvb-usb-digitv.ko.gz
++++++ r/r 911: hid-logitech-dj.ko.gz
++++++ r/r 912: hid-logitech-hidpp.ko.gz
++++++ r/r 913: hid-logitech.ko.gz
++++++ r/r 4323:        nfc_digital.ko.gz
++ d/d 65642:   git-core
++ l/l 65457:   git-receive-pack
++ l/l 65459:   git-upload-archive
++ r/r 65456:   git
++ l/l 65460:   git-upload-pack
++ r/r 65458:   git-shell
++ d/d 65461:   git-core
+++ l/l 65465:  git-annotate
+++ l/l 65471:  git-bugreport
+++ l/l 65474:  git-check-attr
+++ l/l 65485:  git-column
+++ l/l 65500:  git-diff-tree
+++ r/r 65520:  git-http-fetch
+++ l/l 65525:  git-interpret-trailers
+++ l/l 65543:  git-merge-tree
+++ l/l 65551:  git-notes
+++ l/l 65561:  git-range-diff
+++ l/l 65562:  git-read-tree
+++ l/l 65563:  git-rebase
+++ r/r 65585:  git-sh-i18n
+++ l/l 65596:  git-status
+++ l/l 65603:  git-unpack-file
+++ l/l 65478:  git-checkout
+++ l/l 65513:  git-fsmonitor--daemon
+++ l/l 65528:  git-ls-remote
+++ l/l 65533:  git-merge
+++ r/r 65537:  git-merge-octopus
+++ l/l 65552:  git-pack-objects
+++ l/l 65554:  git-pack-refs
+++ l/l 65555:  git-patch-id
+++ l/l 65568:  git-remote-fd
+++ l/l 65580:  git-rev-list
+++ l/l 65609:  git-upload-pack
+++ l/l 65497:  git-diff
+++ l/l 65512:  git-fsck-objects
+++ l/l 65527:  git-ls-files
+++ l/l 65470:  git-branch
+++ l/l 65540:  git-merge-recursive
+++ r/r 65541:  git-merge-resolve
+++ l/l 65547:  git-mktree
+++ l/l 65549:  git-mv
+++ l/l 65578:  git-reset
+++ r/r 65586:  git-sh-i18n--envsubst
+++ l/l 65590:  git-show-branch
+++ l/l 65518:  git-help
+++ l/l 65573:  git-repack
+++ l/l 65583:  git-rm
+++ l/l 65476:  git-check-mailmap
+++ l/l 65604:  git-unpack-objects
+++ l/l 65462:  git
+++ l/l 65479:  git-checkout--worker
+++ l/l 65481:  git-cherry
+++ l/l 65487:  git-commit-graph
+++ r/r 65502:  git-difftool--helper
+++ l/l 65504:  git-fetch
+++ r/r 65521:  git-http-push
+++ l/l 65534:  git-merge-base
+++ l/l 65548:  git-multi-pack-index
+++ l/l 65558:  git-pull
+++ l/l 65569:  git-remote-ftp
+++ l/l 65582:  git-revert
+++ l/l 65591:  git-show-index
+++ l/l 65605:  git-update-index
+++ l/l 65607:  git-update-server-info
+++ l/l 65608:  git-upload-archive
+++ l/l 65509:  git-for-each-repo
+++ l/l 65511:  git-fsck
+++ l/l 65526:  git-log
+++ l/l 65550:  git-name-rev
+++ l/l 65577:  git-rerere
+++ l/l 65581:  git-rev-parse
+++ l/l 65492:  git-credential-cache
+++ l/l 65522:  git-index-pack
+++ l/l 65611:  git-verify-commit
+++ l/l 65616:  git-whatchanged
+++ l/l 65617:  git-worktree
+++ l/l 65464:  git-am
+++ l/l 65488:  git-commit-tree
+++ l/l 65489:  git-config
+++ l/l 65493:  git-credential-cache--daemon
+++ l/l 65498:  git-diff-files
+++ l/l 65469:  git-blame
+++ l/l 65473:  git-cat-file
+++ l/l 65483:  git-clean
+++ l/l 65539:  git-merge-ours
+++ l/l 65542:  git-merge-subtree
+++ l/l 65566:  git-remote
+++ l/l 65588:  git-shortlog
+++ r/r 65598:  git-submodule
+++ l/l 65515:  git-get-tar-commit-id
+++ l/l 65612:  git-verify-pack
+++ l/l 65499:  git-diff-index
+++ l/l 65501:  git-difftool
+++ r/r 65615:  git-web--browse
+++ l/l 65494:  git-credential-store
+++ l/l 65495:  git-describe
+++ l/l 65529:  git-ls-tree
+++ l/l 65535:  git-merge-file
+++ r/r 65538:  git-merge-one-file
+++ l/l 65553:  git-pack-redundant
+++ l/l 65556:  git-prune
+++ l/l 65574:  git-replace
+++ l/l 65575:  git-replay
+++ l/l 65610:  git-var
+++ l/l 65614:  git-version
+++ l/l 65618:  git-write-tree
+++ l/l 65466:  git-apply
+++ l/l 65475:  git-check-ignore
+++ l/l 65477:  git-check-ref-format
+++ l/l 65503:  git-fast-export
+++ l/l 65505:  git-fetch-pack
+++ l/l 65514:  git-gc
+++ l/l 65546:  git-mktag
+++ r/r 65587:  git-sh-setup
+++ l/l 65593:  git-sparse-checkout
+++ l/l 65517:  git-hash-object
+++ l/l 65600:  git-switch
+++ l/l 65482:  git-cherry-pick
+++ l/l 65490:  git-count-objects
+++ l/l 65516:  git-grep
+++ r/r 65560:  git-quiltimport
+++ r/r 65571:  git-remote-http
+++ l/l 65484:  git-clone
+++ l/l 65486:  git-commit
+++ l/l 65491:  git-credential
+++ l/l 65572:  git-remote-https
+++ l/l 65584:  git-send-pack
+++ l/l 65594:  git-stage
+++ l/l 65597:  git-stripspace
+++ l/l 65599:  git-submodule--helper
+++ l/l 65613:  git-verify-tag
+++ r/r 65506:  git-filter-branch
+++ l/l 65559:  git-push
+++ l/l 65564:  git-receive-pack
+++ l/l 65602:  git-tag
+++ l/l 65606:  git-update-ref
+++ l/l 65467:  git-archive
+++ l/l 65507:  git-fmt-merge-msg
+++ l/l 65508:  git-for-each-ref
+++ l/l 65532:  git-maintenance
+++ r/r 65544:  git-mergetool
+++ l/l 65579:  git-restore
+++ l/l 65510:  git-format-patch
+++ l/l 65519:  git-hook
+++ l/l 65523:  git-init
+++ l/l 65480:  git-checkout-index
+++ l/l 65589:  git-show
+++ l/l 65601:  git-symbolic-ref
+++ l/l 65468:  git-bisect
+++ l/l 65472:  git-bundle
+++ l/l 65595:  git-stash
+++ l/l 65565:  git-reflog
+++ l/l 65567:  git-remote-ext
+++ l/l 65530:  git-mailinfo
+++ l/l 65531:  git-mailsplit
+++ l/l 65536:  git-merge-index
+++ r/r 65545:  git-mergetool--lib
+++ l/l 65557:  git-prune-packed
+++ l/l 65570:  git-remote-ftps
+++ r/r 65576:  git-request-pull
+++ l/l 65592:  git-show-ref
+++ l/l 65496:  git-diagnose
+++ l/l 65524:  git-init-db
+++ l/l 65463:  git-add
```
Thứ quan trọng: ++++ d/d 65665: .git
Tức là trong filesystem có repository của Git.
Bài picoCTF kiểu Forensics Git gần như luôn giấu flag trong Git history. Nhưng hiện tại mới thấy .git của hệ thống (git binaries trong /usr/lib/git-core). Ta cần tìm repo của user, thường ở /home.
```
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Forensics Git 0/repo]
└─$ cd home/ctf-player/Code/secrets               
┌──(kali㉿kali)-[~/…/home/ctf-player/Code/secrets]
└─$ ls
note.txt           
┌──(kali㉿kali)-[~/…/home/ctf-player/Code/secrets]
└─$ cat note.txt                   
The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
```
This just Hint of challenge, but I think file have been deleted, and I try see old commit by command:
```
git log -p
```
and I found flag:
```
┌──(kali㉿kali)-[~/…/home/ctf-player/Code/secrets]
└─$ cat note.txt 
The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
                                                                                                                                                                                                                                           
┌──(kali㉿kali)-[~/…/home/ctf-player/Code/secrets]
└─$ git log -p        
commit 327681bb38cf467cec328eec9707b240e3e74ced (HEAD -> master)
Author: ctf-player <ctf-player@example.com>
Date:   Wed Nov 19 08:49:27 2025 +0000

    Wrap this phrase in the flag format: g17_1n_7h3_d15k_041217d8

diff --git a/note.txt b/note.txt
new file mode 100644
index 0000000..46064ac
--- /dev/null
+++ b/note.txt
@@ -0,0 +1 @@
+The picoCTF flag format is 'picoCTF{}' where there is some leetspeak phrase in between the curly braces
```
Actually, we just need try recover the third partittion:
```
┌──(kali㉿kali)-[~/Downloads/picoctf2026/Forensics Git 0]
└─$ mmls disk.img 
DOS Partition Table
Offset Sector: 0
Units are in 512-byte sectors

      Slot      Start        End          Length       Description
000:  Meta      0000000000   0000000000   0000000001   Primary Table (#0)
001:  -------   0000000000   0000002047   0000002048   Unallocated
002:  000:000   0000002048   0000616447   0000614400   Linux (0x83)
003:  000:001   0000616448   0001140735   0000524288   Linux Swap / Solaris x86 (0x82)
004:  000:002   0001140736   0002097151   0000956416   Linux (0x83)
we try: tsk_recover -o 1140736 disk.img extracted
and cd extracted and ls
```
but this just root system and it don't have /home, the home folder is where user data is stored. And we try find .git. We will use fls to list entire filesystem:
```
fls -r -o 1140736 disk.img | grep ".git"
```
And we detect:
```
++++ d/d 65665: .git
```
It is clear sign of reposition Git and we try extract entire data, recover entire filesystem:
```
tsk_recover -o 1140736 disk.img repo
```
And find in repo:
```
cd repo
find . -name ".git"
```
and we see: ./home/ctf-player/Code/secrets/.git
This is poject of user 
Next, we try analys repository Git:
```
cd home/ctf-player/Code/secrets
ls
```
and we see file note.txt. And this is progress. 

