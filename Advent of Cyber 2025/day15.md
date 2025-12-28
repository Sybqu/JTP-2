# Web Attack Forensics - Drone Alone



**Category: 4n6**  
---
# Description

TBFC’s drone scheduler web UI is getting strange, long HTTP requests containing Base64 chunks. Splunk raises an alert: “Apache spawned an unusual process.” On some endpoints, these requests cause the web server to execute shell code, which is obfuscated and hidden within the Base64 payloads. For this room, your job as the Blue Teamer is to triage the incident, identify compromised hosts, extract and decode the payloads and determine the scope.

You’ll use Splunk to pivot between web (Apache) logs and host-level (Sysmon) telemetry.

Follow the investigation steps below; each corresponds to a Splunk query and investigation goal.

## Learning objectives
- Detect and analyze malicious web activity through Apache access and error logs
- Investigate OS-level attacker actions using Sysmon data
- Identify and decode suspicious or obfuscated attacker payloads
- Reconstruct the full attack chain using Splunk for Blue Team investigation


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
