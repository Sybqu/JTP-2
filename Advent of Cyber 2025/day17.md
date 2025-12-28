# CyberChef - Hoperation Save McSkidy


**Category: Cryptography/webex**  
---
# Description

McSkidy is imprisoned in King Malhare's Quantum Warren. Sir BreachBlocker III was put in charge of securing the fortress and implemented several access controls to prevent any escape. His defenses are worthy of his name.

However, McSkidy managed to send vital clues to his team using harmless bunny pictures. One message revealed that five locks needed to be disabled to secure an escape route. The locks can be broken by examining their logic and leveraging the system's built-in chat for the guards. They can be eluded in revealing vital details or even passwords. However, you will need to speak their language.

## Learning objectives
- Introduction to encoding/decoding
- Learn how to use CyberChef
- Identify useful information in web applications through HTTP headers

## Working strat
1. For first challenge normal encoding / decoding b64 back and forth. Also getting info from Network tab under web developer tools
2. For gate 2 same b64 encoding / decoding to and fro but twice this time
3. For gate 3 we ask the AI agent for the password. The password has been xor'd first than b64'd so we just need to decode b64 and XOR it again (XOR'S property) to get the original plaintext
5. For gate 4 its an md5 hash , we go to hash cracker and pass this gate
6. For gate 5 its From B64>From Hex>Reverse same steps
 # Conclusion
```
1st Gate pass: Iamsofluffy
2nd Gate pass: Itoldyoutochangeit!
3rd Gate pass: BugsBunny
4th Gate pass: passw0rd1
5th Gate pass: 51rBR34chBl0ck3r

# Flag: THM{M3D13V4L_D3C0D3R_4D3EO7}
```
