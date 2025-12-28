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
1.Software registry dive in
2. NTUSER HIVE diving in , extracting info
3. Hint: malicious activity started form 21oct 2025
4. We check under uninstall programs and find the only file that was uninstalled on that day 
5. As an attacker i would wanna hide my payload as soon as possible
6. Now heading to comptabability assistant 
7. We locate the path from where the malicious file was run
8. Also i thought the attack would have dumped the payload in /currentversion/run but it wasnt like that


 # Conclusion
```
file name : DroneManager Updater
Execution path : C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe
*Value added to maintain persistence on startup: C:\Program Files\DroneManagar\dronehelper.exe* --background
```
