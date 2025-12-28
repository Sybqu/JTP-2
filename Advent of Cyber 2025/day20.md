# Obfuscation - The Egg Shell File



**Category: Cryptography**  
---
# Description
The Best Festival Company (TBFC) has launched its limited-edition SleighToy, with only ten pieces available at midnight. Within seconds, thousands rushed to buy one, but something strange happened. More than ten lucky customers received confirmation emails stating that their orders were successful. Confusion spread fast. How could everyone have bought the "last" toy? McSkidy was called in to investigate.  

She quickly noticed that multiple buyers purchased at the exact moment, slipping through the system’s timing flaw. Sir Carrotbane’s mischievous Bandit Bunnies had found a way to exploit this chaos, flooding the checkout with rapid clicks. By morning, TBFC faced angry shoppers, missing stock, and a mystery that revealed just how dangerous a few milliseconds could be during the holiday rush.


## Learning objectives
- Understand what race conditions are and how they can affect web applications.
- Learn how to identify and exploit race conditions in web requests.
- How concurrent requests can manipulate stock or transaction values.
- Explore simple mitigation techniques to prevent race condition vulnerabilities.
## Working strat 
1. Different encodings that i've learnt from previous challenges ROT,B64 and XOR are used
2. c2B64 so we deobfuscate and get the url and hence get the flag by running SantaStealer.ps1
3. Now we Deobfuscate the API key. We have to XOR then To Hex
4. Running the validator again and we get the 2nd flag

 # Conclusion
```
flag1: THM{C2_De0bfuscation_29838}
flag2: THM{API_Obfusc4tion_ftw_0283}
```
