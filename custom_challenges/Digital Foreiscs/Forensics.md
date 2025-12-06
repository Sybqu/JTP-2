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
