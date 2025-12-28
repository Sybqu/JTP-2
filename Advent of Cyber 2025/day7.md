# Network Discovery - Scan-ta Clause

**Category: Network Security**  

# Description

#### Christmas preparations are delayed - HopSec has breached our QA environment and locked us out!**<br>
#### Without it, the TBFC projects can't be tested, and our entire SOC-mas pipeline is frozen.<br>
#### To make things worse, the server is slowly transforming into a twisted EAST-mas node.<br>
#### Can you uncover HopSec's trail, find a way back into tbfc-devqa01, and restore the server<br>
#### before the bunny's takeover is complete? For this task, you'll need to check every place to hide,<br>
#### every opened port that bunnies left unprotected. Good luck!
---

## Learning objectives
- Learning basics with nmap
- Learn core network protocols and concepts along the way
- Apply your knowledge to find a way back into the serve


## Working strat
1. NMAP for portscanning the IP Of the machine we are targetting
<img width="1853" height="802" alt="image" src="https://github.com/user-attachments/assets/388b8a3d-b485-4240-a8c9-367b56b7cbd1" />

2. The whole website is infected by "bad bunnies" time to scan the whole port range ( -p- and script=banner arguments helps us to check whats behind the ports)
4. Sus FTP server located and we find key1
5. Switching to TCP scan, and scanning the tbfc app on port 25251 > we get key 2
6. Switching to UDP scan, and scanning other ports, we find a dns server, we can use the <dig> command to perform DNS queries
7. We get our key , now time to remove the bunnies
8. We get database access > switch to listening ports > analyze port 2206 for mysql programs and root privilleges > list tables and get the flag
9. ROT-13 Twunc if u can see it
---
## Conclusion
```
flag: THM{4ll_s3rvice5_d1sc0vered}
```
