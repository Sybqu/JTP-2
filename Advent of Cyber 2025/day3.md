# Splunk Basics


---


## Learning objectives
1.Ingest and interpret custom log data in Splunk

2.Create and apply custom field extractions

3.Use Search Processing Language (SPL) to filter and refine search results

4.Conduct an investigation within Splunk to uncover key insights

## Learning splunk software
1. The logs were already added for us in the splunk software (thanks!)
  
3. Using the various filters we can get either a visual representation of the activity or a statstical account of the activity
  
5. Now we begin our hunt for clues using sql queries or filters

## Log ingestion and log parsing
1. Now based on our filter queries we detect the abnormal logs and note down the abnormal client_ip
2. Cheking out the path field and well the path that has been accessed is where linux stores passwords
```bash
*../../etc/passwd
```
3. Looking for exfiltration attempts <br>
 The attacker exfiltrated varioues files using weget and curl and zgrab<br>
 The attack has gained full access to the database and is now staging the ransomware<br>
  malicious file: bunnylock.bin


### Conclusion
- Identity found: The attacker was identified via the highest volume of malicious web traffic originating from the external IP.
- Intrusion vector: The attack followed a clear progression in the web logs (sourcetype=web_traffic).
- Reconnaissance: Probes were initiated via cURL/Wget, looking for configuration files (/.env) and testing path traversal vulnerabilities.
- Exploitation: The use of SQLmap user agents and specific payloads (SLEEP(5)) confirmed the successful exploitation phase.
- Payload delivery: The Action on Objective was established by the final successful execution of the command cmd=./bunnylock.bin via the webshell.
- C2 confirmation: The pivot to the firewall logs (sourcetype=firewall_logs) proved the post-exploitation activity. The internal, compromised server (SRC_IP: 10.10.1.5) established    an outbound C2 connection to the attacker's IP.
```
Attack ip: 198.51.100.55
Peak traffic day : 2025-10-12
Count of Havij user_agents in logs : 993
Number of  directory traversing attempts : 658
Bytes transferred : 126167 💀


