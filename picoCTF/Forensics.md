# 1. Trivial Flag Transfer Protocol

> Figure out how they moved the flag.
  (enclosed .ncap file)


## Solution:

- Open Wireshark Launch the .ncap file
- We see 2 protocols used ARP and TFTP . Since the challenge itself is named TFTP we have to sniff around TFTP
- I Followed the First packets' UDP stream and it showed me 
  "instructions.txt"
- Hence, i came to the conclusion that there is an object "instructions.txt" in the middle of these 152k some packets
- Extracted objects File>Export Objects>TFTP
- Got Following files


- Opened Instructions , got encrypted text . went to dcode.fr.First guess was caeser or ROT-13. It was ROT-13 .
- Went to "plan" as instructed figured out the password for steganographing the images <DUEDILIGENCE>.
- Intuition: Images: Steganography : Password:DUEDILIIGENCE
- Got the flag

```
put codes & terminal outputs here using triple backticks

you may also use ```python for python codes for example
```

## Flag:

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts learnt:

- TFTP : ~~innocent~~ Primitive version of FTP. 
   - No authentication and encryption
   - Straight up UDP based file transfers
   - Runs on UDP port 69
   - sends and receives chunks of data <blocks> and ACKs
- Exporting objects in Wireshark reassaembles those DATA and ACK packets into the original files
 - It reassembles DATA blocks by ID and order
  - Can be tricky when the packet count is huge as some blocks might be missing if packets were dropped
    - Partial or Corrupt Exports

## Notes:

- Forgot Exporting objects is a thing T__T
-
## Resources:

.

***

# 2. tunn3l v1s10n

> We found this file. Recover the flag.
 (enclosed file )



## Solution:

- Opened the file. Maybe file format issue?  Changed format to jpeg / png didnt work.
- Read file hex for magic bytes

```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Downloads$ xxd tunn3l_v1s10n | head -10
00000000: 424d 8e26 2c00 0000 0000 bad0 0000 bad0  BM.&,...........
00000010: 0000 6e04 0000 3201 0000 0100 1800 0000  ..n...2.........
00000020: 0000 5826 2c00 2516 0000 2516 0000 0000  ..X&,.%...%.....
00000030: 0000 0000 0000 231a 1727 1e1b 2920 1d2a  ......#..'..) .*
00000040: 211e 261d 1a31 2825 352c 2933 2a27 382f  !.&..1(%5,)3*'8/
00000050: 2c2f 2623 332a 262d 2420 3b32 2e32 2925  ,/&#3*&-$ ;2.2)%
00000060: 3027 2333 2a26 382c 2836 2b27 392d 2b2f  0'#3*&8,(6+'9-+/
00000070: 2623 1d12 0e23 1711 2916 0e55 3d31 9776  &#...#..)..U=1.v
00000080: 668b 6652 996d 569e 7058 9e6f 549c 6f54  f.fR.mV.pX.oT.oT
00000090: ab7e 63ba 8c6d bd8a 69c8 9771 c193 71c1  .~c..m..i..q..q.
```
- starts with BM possibly bitmap file.
- Corrupted header <it intentionally says "bad"> T_T
- Looked around BMP and its header details : 
  - BMP FILE header should be 14 bytes and DIB header 40 bytes
   clearly BAD0>>>>>>>>>>>>>>>54 bytes.
- FIX:

```Python
#!/usr/bin/env python3

with open('tunn3l_v1s10n.bmp', 'rb') as f:
    data = bytearray(f.read())

# Fix offset 0x0A: pixel data offset (should be 0x36 = 54)
data[0x0A] = 0x36
data[0x0B] = 0x00
data[0x0C] = 0x00
data[0x0D] = 0x00

# Fix offset 0x0E: DIB header size (should be 0x28 = 40)
data[0x0E] = 0x28
data[0x0F] = 0x00
data[0x10] = 0x00
data[0x11] = 0x00

with open('fixed.bmp', 'wb') as f:
    f.write(data)

print("✅ Fixed BMP saved as fixed.bmp")
print("Width: {} Height: {}".format(
    int.from_bytes(data[0x12:0x16], 'little'),
    int.from_bytes(data[0x16:0x1A], 'little')
))
```
 <i clearly didnt write this script , its too organized for me and i dont know python ^^>

- Fixed the file.
- Hint : File does not look quite right
 - yes this was indeed unsettling but this wasn't my first direction.
- Exiftool showed that file is 2.9 Mb but file size in windows explorer was 2.75 Mb
- New python script to fix width size ><
- Got the flag



## Flag:picoCTF{qu1t3_a_v13w_2020}

## Concepts learnt:

- BMP files
   - Stores pixel data without compression
   - Structure: Header + pixel data
    - 14 byte file header 40 byte variable info header (DIB) + pixel data
    - Learned offset positions , size , name and purpose
 - Magic Bytes <need to memorize ts to some extent i feel>
   - File signatures
   - Hex in a little eldian convention (lsb stored first)
- Python script to fix ts

## Notes:

- Type shit started doing b1nwalk and steganography

## Resources:

Claude my goat

***

