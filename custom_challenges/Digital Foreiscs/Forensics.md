# 1. Hide and Seek

---

##  Challenge Description

description.txt
Description:

Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter.
An old friend hid some secret data in this image. Can you find it before the others do?

Hint:
Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up

---

##  Files Provided

- `sakamoto.jpg` 


## Initial Recon
> Hinted stegseek Challenge <br>
> Downloading stegseek and its dependencies <br>
> Downloading rockyou..txt <Br>



## Exploit strat
>Running stegseek with rockyou.txt on this image <br>
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

Description:

One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right.
Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

---

##  Files Provided

- `nutrela.png` 


## Initial Recon
> Corrupted png file <br>
> Challenge related with fixing the corrupted chunks and signature of the png file <br>

## Vulnerability Analysis
> 1. Opening file in HxD hex editor <br>
>  <img width="915" height="345" alt="image" src="https://github.com/user-attachments/assets/ea5b3d62-4df7-4ca0-a8c4-e2fa8262e1a4" /> <bR>
> 3. Corrupted PNG Magic byes <br>




## Exploit strat
>Fixing the "png" and "ihdr" chunks in HxD <br>
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
> Similarly had to fix IDAT and IDENT chunks <br>
> Also had to fix CRC of the IDAT chunk <br>
> Hence we got the fixed image

## Decoded image
<img width="1000" height="1000" alt="nutrela1" src="https://github.com/user-attachments/assets/0943a471-7c0d-405e-8863-d45ca9950c82" />

```
## Flag: nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```
# 3. RAR of the abyss

---

##  Challenge Description

Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.

---

##  Files Provided

- `abyss.pcap` 


## Initial Recon
> Pcap analysis in Wireshark <br>


## Exploit strat
> Following the TCP streams and getting the password <br>
> <img width="856" height="481" alt="image" src="https://github.com/user-attachments/assets/bcf73ca3-182f-413e-9b9c-28a26636b99d" /> <Br>
```
Password:b3y0ndG00dand3vil
```
> Alongside this extracted RAR magic bytes <br>
<img width="1279" height="543" alt="image" src="https://github.com/user-attachments/assets/7247cf18-7275-410e-93d9-5f03e554ff24" />
<br>
> Converting UTF-8 data to raw bytes and then pasting the bytes in cyberchef to recover X  .RAR fiie <br>
<br>
<img width="1162" height="658" alt="image" src="https://github.com/user-attachments/assets/b8142fde-8b47-4a7a-8554-6e0ccd10742a" />
<Br>
> Enterring pass and getting the flag

```
## Flag: nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```
# 4. Ninetails

---

##  Challenge Description

Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag.

Hint:
I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?

---

##  Files Provided

- `ninetails.rar` 


## Initial Recon
> The .rar inside had .ad1 file <br>
> Loading up the file in FTK imager<br>




## Exploit strat
1. Finding the mozilla appdata folder <br>
2. Path followed : GIC2024/AppData/Roaming/Mozilla/Firefox/profiles/j4gjesg4.default-release <br>
>3. Exporting logins.json and key4.db <Br>
>4. Need firefox_decrypt.py to decrypt the .json data <br>
>5. Downloading from GitHub <br>
>6. Installing dependencies (libnss tools) <br>

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
> volatility --profile=Win7SP1x64 -f memory.raw consoles <br>
  #### flag 1
- volatility --profile=Win7SP1x64 -f memory.raw filescan <br>
#### flag 2
- Provided GIMP file
 1. Needed to set height width and offset of the image
 2. Calculations: File size / 3 bytes (for RGB)
 3. Filter out realistic screen resolutions
 4. Edit offset till the image makes sense
#### flag 3
- While running ps list in the volatility found Winrar.exe running
  1. Expanded on that process that led me to "important.rar" <Br>
  2. Important.rar was password protected <br>
  3. The password was Alyssa's hash in all caps <br>
  4. Used hashdump command to get the hash and complete the challenge

# NOTE 

## BASH DUMP
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
Press any key to continue . . .
**************************************************
ConsoleProcess: conhost.exe Pid: 2260
Console: 0xff756200 CommandHistorySize: 50
HistoryBufferCount: 1 HistoryBufferMax: 4
OriginalTitle: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
Title: C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe
AttachedProcess: DumpIt.exe Pid: 796 Handle: 0x60
----
CommandHistory: 0x38ea90 Application: DumpIt.exe Flags: Allocated
CommandCount: 0 LastAdded: -1 LastDisplayed: -1
FirstCommand: 0 CommandCountMax: 50
ProcessHandle: 0x60
----
Screen 0x371050 X:80 Y:300
Dump:
  DumpIt - v1.3.2.20110401 - One click memory memory dumper
  Copyright (c) 2007 - 2011, Matthieu Suiche <http://www.msuiche.net>
  Copyright (c) 2010 - 2011, MoonSols <http://www.moonsols.com>


    Address space size:        1073676288 bytes (   1023 Mb)
    Free space size:          24185389056 bytes (  23064 Mb)

    * Destination = \??\C:\Users\SmartNet\Downloads\DumpIt\SMARTNET-PC-20191211-
143755.raw

    --> Are you sure you want to continue? [y/n] y
    + Processing...
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 screenshot --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\screenshots"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : Please install PIL
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 screenshot --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\screenshots"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : Please install PIL
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 screenshot --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\screenshots"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : Please install PIL
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 memdump -p 2424 --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\mspaint_dump"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\mspaint_dump is not a directory
PS C:\Users\garri\volatility_2.6_win64_standalone> mkdir C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\mspaint_dump


    Directory: C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        06-12-2025     02:22                mspaint_dump


PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 memdump -p 2424 --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\mspaint_dump"
Volatility Foundation Volatility Framework 2.6
************************************************************************
Writing mspaint.exe [  2424] to 2424.dmp
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 pslist
Volatility Foundation Volatility Framework 2.6
Offset(V)          Name                    PID   PPID   Thds     Hnds   Sess  Wow64 Start                          Exit
------------------ -------------------- ------ ------ ------ -------- ------ ------ ------------------------------ ------------------------------
0xfffffa8000ca0040 System                    4      0     80      570 ------      0 2019-12-11 13:41:25 UTC+0000
0xfffffa800148f040 smss.exe                248      4      3       37 ------      0 2019-12-11 13:41:25 UTC+0000
0xfffffa800154f740 csrss.exe               320    312      9      457      0      0 2019-12-11 13:41:32 UTC+0000
0xfffffa8000ca81e0 csrss.exe               368    360      7      199      1      0 2019-12-11 13:41:33 UTC+0000
0xfffffa8001c45060 psxss.exe               376    248     18      786      0      0 2019-12-11 13:41:33 UTC+0000
0xfffffa8001c5f060 winlogon.exe            416    360      4      118      1      0 2019-12-11 13:41:34 UTC+0000
0xfffffa8001c5f630 wininit.exe             424    312      3       75      0      0 2019-12-11 13:41:34 UTC+0000
0xfffffa8001c98530 services.exe            484    424     13      219      0      0 2019-12-11 13:41:35 UTC+0000
0xfffffa8001ca0580 lsass.exe               492    424      9      764      0      0 2019-12-11 13:41:35 UTC+0000
0xfffffa8001ca4b30 lsm.exe                 500    424     11      185      0      0 2019-12-11 13:41:35 UTC+0000
0xfffffa8001cf4b30 svchost.exe             588    484     11      358      0      0 2019-12-11 13:41:39 UTC+0000
0xfffffa8001d327c0 VBoxService.ex          652    484     13      137      0      0 2019-12-11 13:41:40 UTC+0000
0xfffffa8001d49b30 svchost.exe             720    484      8      279      0      0 2019-12-11 13:41:41 UTC+0000
0xfffffa8001d8c420 svchost.exe             816    484     23      569      0      0 2019-12-11 13:41:42 UTC+0000
0xfffffa8001da5b30 svchost.exe             852    484     28      542      0      0 2019-12-11 13:41:43 UTC+0000
0xfffffa8001da96c0 svchost.exe             876    484     32      941      0      0 2019-12-11 13:41:43 UTC+0000
0xfffffa8001e1bb30 svchost.exe             472    484     19      476      0      0 2019-12-11 13:41:47 UTC+0000
0xfffffa8001e50b30 svchost.exe            1044    484     14      366      0      0 2019-12-11 13:41:48 UTC+0000
0xfffffa8001eba230 spoolsv.exe            1208    484     13      282      0      0 2019-12-11 13:41:51 UTC+0000
0xfffffa8001eda060 svchost.exe            1248    484     19      313      0      0 2019-12-11 13:41:52 UTC+0000
0xfffffa8001f58890 svchost.exe            1372    484     22      295      0      0 2019-12-11 13:41:54 UTC+0000
0xfffffa8001f91b30 TCPSVCS.EXE            1416    484      4       97      0      0 2019-12-11 13:41:55 UTC+0000
0xfffffa8000d3c400 sppsvc.exe             1508    484      4      141      0      0 2019-12-11 14:16:06 UTC+0000
0xfffffa8001c38580 svchost.exe             948    484     13      322      0      0 2019-12-11 14:16:07 UTC+0000
0xfffffa8002170630 wmpnetwk.exe           1856    484     16      451      0      0 2019-12-11 14:16:08 UTC+0000
0xfffffa8001d376f0 SearchIndexer.          480    484     14      701      0      0 2019-12-11 14:16:09 UTC+0000
0xfffffa8001eb47f0 taskhost.exe            296    484      8      151      1      0 2019-12-11 14:32:24 UTC+0000
0xfffffa8001dfa910 dwm.exe                1988    852      5       72      1      0 2019-12-11 14:32:25 UTC+0000
0xfffffa8002046960 explorer.exe            604   2016     33      927      1      0 2019-12-11 14:32:25 UTC+0000
0xfffffa80021c75d0 VBoxTray.exe           1844    604     11      140      1      0 2019-12-11 14:32:35 UTC+0000
0xfffffa80021da060 audiodg.exe            2064    816      6      131      0      0 2019-12-11 14:32:37 UTC+0000
0xfffffa80022199e0 svchost.exe            2368    484      9      365      0      0 2019-12-11 14:32:51 UTC+0000
0xfffffa8002222780 cmd.exe                1984    604      1       21      1      0 2019-12-11 14:34:54 UTC+0000
0xfffffa8002227140 conhost.exe            2692    368      2       50      1      0 2019-12-11 14:34:54 UTC+0000
0xfffffa80022bab30 mspaint.exe            2424    604      6      128      1      0 2019-12-11 14:35:14 UTC+0000
0xfffffa8000eac770 svchost.exe            2660    484      6      100      0      0 2019-12-11 14:35:14 UTC+0000
0xfffffa8001e68060 csrss.exe              2760   2680      7      172      2      0 2019-12-11 14:37:05 UTC+0000
0xfffffa8000ecbb30 winlogon.exe           2808   2680      4      119      2      0 2019-12-11 14:37:05 UTC+0000
0xfffffa8000f3aab0 taskhost.exe           2908    484      9      158      2      0 2019-12-11 14:37:13 UTC+0000
0xfffffa8000f4db30 dwm.exe                3004    852      5       72      2      0 2019-12-11 14:37:14 UTC+0000
0xfffffa8000f4c670 explorer.exe           2504   3000     34      825      2      0 2019-12-11 14:37:14 UTC+0000
0xfffffa8000f9a4e0 VBoxTray.exe           2304   2504     14      144      2      0 2019-12-11 14:37:14 UTC+0000
0xfffffa8000fff630 SearchProtocol         2524    480      7      226      2      0 2019-12-11 14:37:21 UTC+0000
0xfffffa8000ecea60 SearchFilterHo         1720    480      5       90      0      0 2019-12-11 14:37:21 UTC+0000
0xfffffa8001010b30 WinRAR.exe             1512   2504      6      207      2      0 2019-12-11 14:37:23 UTC+0000
0xfffffa8001020b30 SearchProtocol         2868    480      8      279      0      0 2019-12-11 14:37:23 UTC+0000
0xfffffa8001048060 DumpIt.exe              796    604      2       45      1      1 2019-12-11 14:37:54 UTC+0000
0xfffffa800104a780 conhost.exe            2260    368      2       50      1      0 2019-12-11 14:37:54 UTC+0000
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 dumpfiles -Q 0xYOURADDRESS --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump is not a directory
PS C:\Users\garri\volatility_2.6_win64_standalone> mkdir  C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump


    Directory: C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----        06-12-2025     02:56                rar_dump


PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 dumpfiles -Q 0xYOURADDRESS --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : Invalid PHYSOFFSET 0xYOURADDRESS
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 dumpfiles -Q 0xYOURADDRESS --dump-dir "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : Invalid PHYSOFFSET 0xYOURADDRESS
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 cmdline
Volatility Foundation Volatility Framework 2.6
************************************************************************
System pid:      4
************************************************************************
smss.exe pid:    248
Command line : \SystemRoot\System32\smss.exe
************************************************************************
csrss.exe pid:    320
Command line : %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitialization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
************************************************************************
csrss.exe pid:    368
Command line : %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitialization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
************************************************************************
psxss.exe pid:    376
Command line : %SystemRoot%\system32\psxss.exe
************************************************************************
winlogon.exe pid:    416
Command line : winlogon.exe
************************************************************************
wininit.exe pid:    424
Command line : wininit.exe
************************************************************************
services.exe pid:    484
Command line : C:\Windows\system32\services.exe
************************************************************************
lsass.exe pid:    492
Command line : C:\Windows\system32\lsass.exe
************************************************************************
lsm.exe pid:    500
Command line : C:\Windows\system32\lsm.exe
************************************************************************
svchost.exe pid:    588
Command line : C:\Windows\system32\svchost.exe -k DcomLaunch
************************************************************************
VBoxService.ex pid:    652
Command line : C:\Windows\System32\VBoxService.exe
************************************************************************
svchost.exe pid:    720
Command line : C:\Windows\system32\svchost.exe -k RPCSS
************************************************************************
svchost.exe pid:    816
Command line : C:\Windows\System32\svchost.exe -k LocalServiceNetworkRestricted
************************************************************************
svchost.exe pid:    852
Command line : C:\Windows\System32\svchost.exe -k LocalSystemNetworkRestricted
************************************************************************
svchost.exe pid:    876
Command line : C:\Windows\system32\svchost.exe -k netsvcs
************************************************************************
svchost.exe pid:    472
Command line : C:\Windows\system32\svchost.exe -k LocalService
************************************************************************
svchost.exe pid:   1044
Command line : C:\Windows\system32\svchost.exe -k NetworkService
************************************************************************
spoolsv.exe pid:   1208
Command line : C:\Windows\System32\spoolsv.exe
************************************************************************
svchost.exe pid:   1248
Command line : C:\Windows\system32\svchost.exe -k LocalServiceNoNetwork
************************************************************************
svchost.exe pid:   1372
Command line : C:\Windows\system32\svchost.exe -k LocalServiceAndNoImpersonation
************************************************************************
TCPSVCS.EXE pid:   1416
Command line : C:\Windows\System32\tcpsvcs.exe
************************************************************************
sppsvc.exe pid:   1508
Command line : C:\Windows\system32\sppsvc.exe
************************************************************************
svchost.exe pid:    948
Command line : C:\Windows\System32\svchost.exe -k secsvcs
************************************************************************
wmpnetwk.exe pid:   1856
Command line : "C:\Program Files\Windows Media Player\wmpnetwk.exe"
************************************************************************
SearchIndexer. pid:    480
Command line : C:\Windows\system32\SearchIndexer.exe /Embedding
************************************************************************
taskhost.exe pid:    296
Command line : "taskhost.exe"
************************************************************************
dwm.exe pid:   1988
Command line : "C:\Windows\system32\Dwm.exe"
************************************************************************
explorer.exe pid:    604
Command line : C:\Windows\Explorer.EXE
************************************************************************
VBoxTray.exe pid:   1844
Command line : "C:\Windows\System32\VBoxTray.exe"
************************************************************************
audiodg.exe pid:   2064
Command line : C:\Windows\system32\AUDIODG.EXE 0x20c
************************************************************************
svchost.exe pid:   2368
Command line : C:\Windows\System32\svchost.exe -k LocalServicePeerNet
************************************************************************
cmd.exe pid:   1984
Command line : "C:\Windows\system32\cmd.exe"
************************************************************************
conhost.exe pid:   2692
Command line : \??\C:\Windows\system32\conhost.exe
************************************************************************
mspaint.exe pid:   2424
Command line : "C:\Windows\system32\mspaint.exe"
************************************************************************
svchost.exe pid:   2660
Command line : C:\Windows\system32\svchost.exe -k imgsvc
************************************************************************
csrss.exe pid:   2760
Command line : %SystemRoot%\system32\csrss.exe ObjectDirectory=\Windows SharedSection=1024,20480,768 Windows=On SubSystemType=Windows ServerDll=basesrv,1 ServerDll=winsrv:UserServerDllInitialization,3 ServerDll=winsrv:ConServerDllInitialization,2 ServerDll=sxssrv,4 ProfileControl=Off MaxRequestThreads=16
************************************************************************
winlogon.exe pid:   2808
Command line : winlogon.exe
************************************************************************
taskhost.exe pid:   2908
Command line : "taskhost.exe"
************************************************************************
dwm.exe pid:   3004
Command line : "C:\Windows\system32\Dwm.exe"
************************************************************************
explorer.exe pid:   2504
Command line : C:\Windows\Explorer.EXE
************************************************************************
VBoxTray.exe pid:   2304
Command line : "C:\Windows\System32\VBoxTray.exe"
************************************************************************
SearchProtocol pid:   2524
Command line : "C:\Windows\system32\SearchProtocolHost.exe" Global\UsGthrFltPipeMssGthrPipe_S-1-5-21-3073570648-3149397540-2269648332-10032_ Global\UsGthrCtrlFltPipeMssGthrPipe_S-1-5-21-3073570648-3149397540-2269648332-10032 1 -2147483646 "Software\Microsoft\Windows Search" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT; MS Search 4.0 Robot)" "C:\ProgramData\Microsoft\Search\Data\Temp\usgthrsvc" "DownLevelDaemon"  "1"
************************************************************************
SearchFilterHo pid:   1720
Command line : "C:\Windows\system32\SearchFilterHost.exe" 0 508 512 520 65536 516
************************************************************************
WinRAR.exe pid:   1512
Command line : "C:\Program Files\WinRAR\WinRAR.exe" "C:\Users\Alissa Simpson\Documents\Important.rar"
************************************************************************
SearchProtocol pid:   2868
Command line : "C:\Windows\system32\SearchProtocolHost.exe" Global\UsGthrFltPipeMssGthrPipe3_ Global\UsGthrCtrlFltPipeMssGthrPipe3 1 -2147483646 "Software\Microsoft\Windows Search" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT; MS Search 4.0 Robot)" "C:\ProgramData\Microsoft\Search\Data\Temp\usgthrsvc" "DownLevelDaemon"
************************************************************************
DumpIt.exe pid:    796
Command line : "C:\Users\SmartNet\Downloads\DumpIt\DumpIt.exe"
************************************************************************
conhost.exe pid:   2260
Command line : \??\C:\Windows\system32\conhost.exe
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 filescan | findstr "Important"
Volatility Foundation Volatility Framework 2.6
0x000000003fa3ebc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fac3bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
0x000000003fb48bc0      1      0 R--r-- \Device\HarddiskVolume2\Users\Alissa Simpson\Documents\Important.rar
PS C:\Users\garri\volatility_2.6_win64_standalone> .\volatility_2.6_win64_standalone.exe -f "C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\MemoryDump_Lab1.raw" --profile=Win7SP1x64 dumpfiles -Q 0x000000003e8a5250 --dump-dir "C:\User0x000000003fa3ebc0s\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump"
Volatility Foundation Volatility Framework 2.6
ERROR   : volatility.debug    : C:\User0x000000003fa3ebc0s\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump is not a directory
PS C:\Users\garri\volatility_2.6_win64_standalone> mkdir C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump
mkdir : An item with the specified name C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump already exists.
At line:1 char:1
+ mkdir C:\Users\garri\Desktop\JTP-2\Forensics\chal5\ReDraw\rar_dump
+ ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
    + CategoryInfo          : ResourceExists: (C:\Users\garri\...ReDraw\rar_dump:String) [New-Item], IOException
    + FullyQualifiedErrorId : DirectoryExist,Microsoft.PowerShell.Commands.NewItemCommand

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
```
## Flag1: flag{th1s_1s_th3_1st_st4g3!!}

## Flag2: flag{G00d_BoY_good_girl}

## Flag3: flag{w3ll_3rd_stage_was_easy}
