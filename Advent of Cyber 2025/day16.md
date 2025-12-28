# Forensics - Registry Furensics

**Category: 4n6**  
---
# Description

TBFC is under attack. Systems are exhibiting weird behavior, and the company is now feeling the absence of its lead defender, McSkidy. However, McSkidy made sure the legacy continues.

McSkidy’s team, determined and well-trained, is fully confident in securing all the systems and regaining control before the big event, SOCMAS.

They have now decided to conduct a detailed forensic analysis on one of the most critical systems of TBFC, dispatch-srv01. The dispatch-srv01 coordinates the drone-based gifts delivery during SOCMAS. However, recently it was compromised by King Malhare’s bandits of bunnies.

TBFC’s defenders have decided to split into specialized teams to uncover the attack on this system through detailed forensics. While some of the other team members investigate logs, memory dumps, file systems, and other artefacts, you will work to investigate the registry of this compromised system.

## Learning objectives
- Understand what the Windows Registry is and what it contains.
- Dive deep into Registry Hives and Root Keys.
- Analyze Registry Hives through the built-in Registry Editor tool.
- Learn Registry Forensics and investigate through the Registry Explorer tool.


## Working strat
1. Hop on splunk
2. Allat splunk stuff , statistical representation , visual representation , queries etc. to find out the attack window
3. The challenge tells us to search for suspicious web commands
4. Long B64 encoding strings
5. Cyber-chef!!!
6. "This is now mine" it said
7. Switching to apache error logs > More b64 strings
8. Sysmon logs now.
<img width="938" height="456" alt="image" src="https://github.com/user-attachments/assets/e647201f-b19b-42d4-988d-1d214f72255f" />

9. Apache is spawning cmd.exe
10. Confirming Attacker Enumeration Activity <br>
 "" Attackers often use the whoami command immediately after gaining code execution to determine which user account their malicious process is running as.
       Finding these events confirms the attacker’s post-exploitation reconnaissance, showing that the injected command was executed on the host. ""


    # Conclusion
```
file name : whoami.exe
Attempted executable through injection : powershell.exe

```
