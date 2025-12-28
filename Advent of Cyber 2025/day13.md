# YARA Rules - YARA mean one!


**Category:Forensics**  
---
# Description

When McSkidy went missing, there was chaos and uncertainty at The Best Festival Company (TBFC).
However, even in her disappearance, McSkidy was trying to help the TBFC blue team. 
Taking a page out of the crisis communication process, McSkidy sent what looks like a bunch of images to the blue team from an anonymous location.
These images looked like they were related to Easter preparations, but they contained a message sent by McSkidy. The crisis communication process
outlines that a message might be sent through a folder of images containing hidden messages that can be decoded if you know the keyword. The blue
team has to create a YARA rule that runs on the directory containing the images. The YARA rule must trigger on a keyword followed by a code word.
After extracting all the code words in ascending order, the blue team will be able to decode the message.


---

## Learning objectives
- Understand the basic concept of YARA.
- Learn when and why we need to use YARA rules.
- Explore different types of YARA rules.
- Learn how to write YARA rules.
- Practically detect malicious indicators using YARA.


## Working strat
1. Crafting custom YARA RULE
 ```YARA RULE
rule TBFC_Simple
{
    meta:
        author = "user"
        description = "detect info in images"
        date = "2025-10-10"
    
    strings:
    $tbfc_msg = /TBFC:[A-Za-z0-9]+/ascii

    condition:
    $tbfc_msg
}
 ```
3. Regex expressions and YARA. Realized this is almost like globbing i suppose. /TBFC:[A-Za-z0-9]+/ works.
4. When we use YARA to run our custom rule we get 5 .jpg results
5. Find me in HopSec island they say

## Conclusion
```
- 5 .jpg images
-  /TBFC:[A-Za-z0-9]+/ Regex that works
- Find me in HopSec Island - secret message
```
