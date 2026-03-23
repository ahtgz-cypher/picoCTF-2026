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
Rogue Tower
Author: Samuel Dinesh

Description
A suspicious cell tower has been detected in the network. Analyze the captured network traffic to identify the rogue tower, find the compromised device, and recover the exfiltrated flag.
Download the network capture file: here https://challenge-files.picoctf.net/c_plain_mesa/92affee6c1332c869e9f2e0184dc78490f3a166a606b42f2654fb713e43f4217/rogue_tower.pcap
Hints 
Look for unauthorized test network broadcasts on UDP port 55000
Find the device that connected to the rogue tower by checking HTTP User-Agent headers
The encryption key is derived from the victim device's IMSI
The exfiltrated data is split across multiple HTTP POST requests

```

### Forensics

#### 🔐 Challenge 1: Easy Cipher
- 🏷️ Category: Forensics
- 🔥 Difficulty: Medium

**📖 Description:**
First, I open file pcap with wireshark, and I take a look trafics, I realize in the seventeen trafic, it has act upload something similar with base64 and i try take it out and put it back together and i has string:
```
RVlUW3VsdEJHAFBBBWdRCllcaEAGTwFLagNWAVINB1sHTQ==
```
And i try decode it into base64 and save it into file data.bin by command:
```
echo "RVlUW3VsdEJHAFBBBWdRCllcaEAGTwFLagNWAVINB1sHTQ==" | base64 -d > data.bin
```
After i read Hint: Look for unauthorized test network broadcasts on UDP port 55000 
And I try filter:
```
udp.port == 55000
```
And I see: 
```
CARRIER: Verizon        PLMN=310410 CELLID=14884
CARRIER: AT&T           PLMN=310410 CELLID=14885
UNAUTHORIZED-TEST-NETWORK PLMN=00101 CELLID=91521
```
So, Rogue Tower in this is CELLID=91521
And i continue read Hint: Find the device that connected to the rogue tower, and i try filter HTTP. I try observe header:
```
User-Agent: MobileDevice/1.0 (IMSI:310410050746829; CELL:91521)
```
And i have IMSI:310410050746829. It is about this device connect rogue tower and it is victim
Next i read Hint: The encryption key is derived from the victim device's IMSI
I try the ISMI variants and key right is:
```
IMSI[-8:] = 50746829
```
I xor to decode it:
```
import base64

data = base64.b64decode("RVlUW3VsdEJHAFBBBWdRCllcaEAGTwFLagNWAVINB1sHTQ==")
key = b"50746829"

out = bytes(data[i] ^ key[i % len(key)] for i in range(len(data)))
print(out)
```
and result: picoCTF{r0gu3_c3ll_t0w3r_3a5d55b2}
