# Passwords - A Cracking Christmas

**Category:Cryptography**  

# Description

With time between Easter and Christmas being destabilised, the once-quiet systems of The Best Festival Company began showing traces of encrypted data buried deep within their servers. Sir Carrotbane, stumbled upon a series of locked PDF and ZIP files labelled “North Pole Asset List.” Rumours spread that these could contain fragments of Santa’s master gift registry, critical information that could help Malhare control the festive balance between both worlds.

Sir Carrotbane sets out to crack the encryption, learning how weak passwords can expose even the most guarded secrets. Can the Elves adapt fast and prevent their secrets from being discovered?

---

## Learning objectives
-How password-based encryption protects files such as PDFs and ZIP archives.
-Why weak passwords make encrypted files vulnerable.
-How attackers use dictionary and brute-force attacks to recover passwords.
-A hands-on exercise: cracking the password of an encrypted file to reveal its contents.
-The importance of using strong, complex passwords to defend against these attacks.

## Working strat
1. Use pdfcrack tool and use rockyou.txt like that one steg challenge from custom challenges
2. Pdf file pass : naughtylist , first flag.
3. Cracking the ZIP file using john the ripper
4. ```
   zip2john flag.zip > ziphash.txt
   ```

---
## Conclusion
```
flag1: THM{Cr4ack1ng_PDFs_1s_34$sy}
flag2: THM{Cr4ack1ng_z1p$_1s_34$syyyy}
```
