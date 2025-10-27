# 1. gdb baby step 1

> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.
Disassemble this.
included a file.
## Solution:

Since the challenge is "Gdb baby step 1" i thought of using gdb
did it through that too but i was itching to try this through IDA
so yeah i did. 
Loaded the file into IDA, it recognized the file as an ELF
From the list of functions selected "main" located "eax" and then the flag was right next to it in hex
converted it to decimal and boom the flag is mine.

```

```

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- Learning Basic IDA
- Differences between IDA and GDB

## Notes:

- Initially IDA wasnt recognizing the given file as PE / ELF file and loading it as raw binary so i had to "look around" and yeah it recognized it eventually

## Resources:



***

# 2.ARMseembly1

> Description
For what argument does this program print `win` with variables 87, 3 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:
Yeah so learning quite a bit of assembly.
Checked the code analyzed the commands
Took sweet time checking out the commands.
The header head general info of the code :3

the function was mentioned before the main function very alike to modular programming
Searched up the various commands present and their syntaxes
Involvment of pointers and loading them into registers via ldr command

Also the code had blocks LO-L6 apparently used to execute commands together (maybe an assembly thing?)
Pseudocode:
Enter argument > code executes (general mathematics and bitwise shifting)>return result > result = 232 - arg> result ==0 for win else we lose.

## Flag:

```
picoCTF{000000e8}

```
## Concepts learnt:

- Basics of Assembly

## Notes:
- Using pen and paper would've actually been faster than just rawdogging it.

# 3. Vault-door-3

> Description
This vault uses for-loops and byte arrays. The source code for this vault is here: VaultDoor3.java

## Solution:
Opened the code up
Initial declarations about verifiying the password.

Main function we are concerend with:
We are given a buffer: "jU5t_a_sna_3lpm12g94c_u_4_m7ra41"
and we are given the Password array;
The multiple for loops assign the characters of our buffer to the password by rotating it using for loops
Solved by writing buffer [i] : password [i] positions:
From the loops you get the Pattern
buffer → password mapping:
buffer[0-7]     < password[0-7]
buffer[8-15]    < password[15-8] 
buffer[16]      < password[30]
..... and so on according to directions provided
.
Then comparing password[i] to respective mapped buffer value

## Simpler explanation(i couldnt put this into words properly myself): (AI):
The Problem:

Program scrambles your password into: "jU5t_a_sna_3lpm12g94c_u_4_m7ra41"
Need to find the original password

How it scrambles:

First 8 chars → stay same
Next 8 chars → reversed
Last 16 chars → shuffled with pattern

Solution:

Work backwards from the scrambled buffer
Map each buffer position back to original password position
Unscramble to get password.

## Flag:

```
picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_c79a21}

```
## Concepts learnt:

- Basic Java and problem solving

## Notes:
:3 