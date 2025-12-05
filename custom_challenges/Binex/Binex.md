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


