# 1. Hide and Seek

---

##  Challenge Description

> Description:
 Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter.
 An old friend hid some secret data in this image. Can you find it before the others do?
 Hint:
 Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up

---

##  Files Provided

- `sakamoto.jpg` 


## Initial Recon
- Hinted stegseek Challenge <br>
- Downloading stegseek and its dependencies <br>
- Downloading rockyou..txt <Br>



## Exploit strat
- Running stegseek with rockyou.txt on this image <br>
```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/Forensics/chal1$ stegseek sakamoto.jpg /usr/share/wordlists/rocky
ou.txt
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".
the file "sakamoto.jpg.out" does already exist. overwrite ? (y/n)
```
```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/Forensics/chal1$ file sakamoto.jpg.out
sakamoto.jpg.out: ASCII text
```

```
## Flag: nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}

```
# 2. Nutrela chunks

---

##  Challenge Description

> Description:
 One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right.
 Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

---

##  Files Provided

- `nutrela.png` 


## Initial Recon
1. Corrupted png file <br>
2. Challenge related with fixing the corrupted chunks and signature of the png file <br>

## Vulnerability Analysis
1. Opening file in HxD hex editor <br>

  <img width="915" height="345" alt="image" src="https://github.com/user-attachments/assets/ea5b3d62-4df7-4ca0-a8c4-e2fa8262e1a4" /> <bR>
  
2. Corrupted PNG Magic byes <br>


## Exploit strat
1. Fixing the "png" and "ihdr" chunks in HxD <br>
 > File is still not fixed <br>

```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/Forensics/chal2$ pngcheck -vt nutrela.png
zlib warning:  different version (expected 1.2.13, using 1.3)

File: nutrela.png (538662 bytes)
  chunk ihdr at offset 0x0000c, length 13:  first chunk must be IHDR
ERRORS DETECTED in nutrela.png
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/Forensics/chal2$
File: nutrela.png (538662 bytes)
  chunk IHDR at offset 0x0000c, length 13
    1000 x 1000 image, 24-bit RGB, non-interlaced
  chunk idat at offset 0x00025, length 538605:  illegal reserved-bit-set chunk
ERRORS DETECTED in nutrela.png

```
2. Similarly had to fix IDAT and IDENT chunks <br>
3. Also had to fix CRC of the IDAT chunk <br> <! Lol i guess im the one who probably messed this up in the first place -->
4. Hence we got the fixed image

## Decoded image
<img width="1000" height="1000" alt="nutrela1" src="https://github.com/user-attachments/assets/0943a471-7c0d-405e-8863-d45ca9950c82" />

```
## Flag: nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```
# 3. RAR of the abyss

---

##  Challenge Description

> Description:
 Two philosophers peer into the networked abyss and swap a secret.
 Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.

---

##  Files Provided

- `abyss.pcap` 


## Initial Recon
> Pcap analysis in Wireshark <br>


## Exploit strat
1. Following the TCP streams and getting the password <br>
 
 <img width="856" height="481" alt="image" src="https://github.com/user-attachments/assets/bcf73ca3-182f-413e-9b9c-28a26636b99d" /> <Br>
 
```
Password:b3y0ndG00dand3vil
```
2. Alongside this extracted RAR magic bytes <br>
<img width="1279" height="543" alt="image" src="https://github.com/user-attachments/assets/7247cf18-7275-410e-93d9-5f03e554ff24" />
<br>
3. Converting UTF-8 data to raw bytes and then pasting the bytes in cyberchef to recover X  .RAR fiie <br>
<br>

<img width="1162" height="658" alt="image" src="https://github.com/user-attachments/assets/b8142fde-8b47-4a7a-8554-6e0ccd10742a" />

<Br>
4. Enterring pass and getting the flag

```
## Flag: nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```
# 4. Ninetails

---

##  Challenge Description

> Description:
 Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide  you to the flag.

 Hint:
 I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?

---

##  Files Provided

- `ninetails.rar` 


## Initial Recon
1. The .rar inside had .ad1 file <br>
2. Loading up the file in FTK imager<br>




## Exploit strat
1. Finding the mozilla appdata folder <br>
2. Path followed : GIC2024/AppData/Roaming/Mozilla/Firefox/profiles/j4gjesg4.default-release <br>
3. Exporting logins.json and key4.db <Br>
4. Need firefox_decrypt.py to decrypt the .json data <br>
5. Downloading from GitHub <br>
6. Installing dependencies (libnss tools) <br>

```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/Forensics/chal4$ export LD_LIBRARY_PATH=/usr/lib/x86_64-linux-gnu python3 firefox_decrypt.py .
2025-12-05 18:50:58,866 - WARNING - profile.ini not found in .
2025-12-05 18:50:58,867 - WARNING - Continuing and assuming '.' is a profile location

Website:  https://www.rehack.xyz
Username: 'warlocksmurf'
Password: 'GCTF{m0zarella'

Website:  https://ctftime.org
Username: 'ilovecheese'
Password: 'CHEEEEEEEEEEEEEEEEEEEEEEEEEESE'

Website:  https://www.reddit.com
Username: 'bluelobster'
Password: '_f1ref0x_'

Website:  https://www.facebook.com
Username: 'flag'
Password: 'SIKE'

Website:  https://warlocksmurf.github.io
Username: 'Man I Love Forensics'
Password: 'p4ssw0rd}'
```


```
## Flag: GCTF{m0zarellaCHEEEEEEEEEEEEEEEEEEEEEEEEEESE_f1ref0x_SIKEp4ssw0rd}

```
# 5. Re:Draw
---

##  Challenge Description

> Description
 A system suddenly crashed after a black command window briefly appeared.  
 A memory dump was captured at the moment of failure.

 The challenge contains **THREE FLAGS**, each hidden in different layers of RAM:

- **Flag 1:** Terminal output before crash
- **Flag 2:** Remnants of a painting/drawing program
- **Flag 3:** A hidden archive buried deeper in memory

 Hint: Learn and use **Volatility 2 plugins**.

---

#  Tools Used

- **Volatility 2**

---

#  Step 1 — Identify Memory Profile

```bash
volatility -f memory.raw imageinfo
```
> This command returns details about the RAM image: <Br>

Windows version <Br>

Architecture <br>

Suggested Volatility profile (e.g., Win7SP1x64) <Br>

# 1. Extraction strat
### Flag 1
- volatility --profile=Win7SP1x64 -f memory.raw consoles <br>
- volatility --profile=Win7SP1x64 -f memory.raw filescan <br>
```bash
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 consoles
Volatility Foundation Volatility Framework 2.6
**************************************************
ConsoleProcess: conhost.exe Pid: 2692
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: %SystemRoot%\system32\cmd.exe
Title: C:\Windows\system32\cmd.exe - St4G3$1
AttachedProcess: cmd.exe Pid: 1984 Handle: 0x60
----
CommandHistory: 0x1fe9c0 Application: cmd.exe Flags: Allocated, Reset
CommandCount: 1 LastAdded: 0 LastDisplayed: 0
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
Cmd #0 at 0x1de3c0: St4G3$1
----
Screen 0x1e0f70 X:80 Y:300
Dump:
Microsoft Windows [Version 6.1.7601]
Copyright (c) 2009 Microsoft Corporation.  All rights reserved.

C:\Users\SmartNet>St4G3$1
ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0=
```
- `ZmxhZ3t0aDFzXzFzX3RoM18xc3Rfc3Q0ZzMhIX0= `: base 64 string
- Decoding it via cyberchef to get Flag 1


#### flag 2
`0xfffffa80022bab30 mspaint.exe            2424    604      6      128      1      0 2019-12-11 14:35:14 UTC+0000`
```bash
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 memdump -p 2424 --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\mspaint_dump"
```
- Using the screenshot plugin to recreate psuedo screenshot by GDI information
- Opening it with GIMP
 1. The image provided was obscure , we needed to find correct dimensions to read it
 2. Filtering in realistic screen resolutions to read the flag 3


    <img width="830" height="329" alt="image" src="https://github.com/user-attachments/assets/3bd6ed7d-dfba-4079-bd5e-c6b64cad9802" />

 3. The dimensions that worked for me were 1640*(1400 -) . I still cant figure out why width works at 1640 specifically
#### flag 3
```bash
WinRAR.exe pid:   1512
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\Alissa Simpson\Documents\Important.rar"
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 dumpfiles -Q 0x000000003fa3ebc0 --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump"
Volatility Foundation Volatility Framework 2.6
DataSectionObject 0x3fa3ebc0   None   \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 hashdump
Volatility Foundation Volatility Framework 2.6
Administrator:500:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
Guest:501:aad3b435b51404eeaad3b435b51404ee:31d6cfe0d16ae931b73c59d7e0c089c0:::
SmartNet:1001:aad3b435b51404eeaad3b435b51404ee:4943abb39473a6f32c11301f4987e7e0:::
HomeGroupUser$:1002:aad3b435b51404eeaad3b435b51404ee:f0fc3d257814e08fea06e63c5762ebd5:::
Alissa Simpson:1003:aad3b435b51404eeaad3b435b51404ee:f4ff64c8baac57d22f22edc681055ba6:::
```
- While running ps list in the volatility found Winrar.exe running
  1. Expanded on that process that led me to "important.rar" <Br>
  2. Important.rar was password protected <br>
  3. The password was Alyssa's hash in all caps <br>
  4. Used hashdump command to get the hash and complete the challenge

  <img width="503" height="494" alt="image" src="https://github.com/user-attachments/assets/1cf55c55-b097-4292-be70-a44519a4a0ce" />


```
## Flag1: flag{th1s_1s_th3_1st_st4g3!!}

## Flag2: flag{G00d_BoY_good_girl}

## Flag3: flag{w3ll_3rd_stage_was_easy}
```
