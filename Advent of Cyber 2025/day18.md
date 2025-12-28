# Obfuscation - The Egg Shell File



**Category: Cryptography**  
---
# Description
WareVille has not felt right since the wormhole appeared. Everyone in TBFC is on high alert: Systems are going haywire, dashboards are spiking, and SOC alerts have been firing nonstop. Amid the chaos, McSkidy keeps her focus on a particular alert that caught her interest: an email posing as northpole-hr. It’s littered with carrot emojis, but the weird thing is this: there is no North Pole human resources department. TBFC’s HR is at theSouth Pole.

Digging further she found a tiny PowerShell file from that email was downloaded. Among the code are random strings of characters, all gibberish, nothing readable at a glance.

McSkidy knows malicious actors often hide code and data using a technique called obfuscation. But what is it, really? And how can we decipher it?

## Learning objectives
- Learn about obfuscation, why and where it is used.
- Learn the difference between encoding, encryption, and obfuscation.
- Learn about obfuscation and the common techniques.
- Use CyberChef to recover plaintext safely.

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
