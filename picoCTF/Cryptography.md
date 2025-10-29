# 1. RSA ORACLE

> Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
Additional details will be available after launching your challenge instance.

## Solution:

- RSA encryption knowledge required
- Finding the public key
- A Bit of mathematics
 - c = m^e mod n
  m^e - c is divisble by n
   Also the corollary of the RSA algorithm GCD of e and m^e - c needs to be n

- We run plain text attack to get values of e and n (the code for which i did not write again..because i suck at ts...even having trouble understanding but we hard)

```python

from Crypto.Util.number import GCD, bytes_to_long, long_to_bytes, inverse
import socket
import time

def interact_oracle(message):
    """Send message to oracle and get ciphertext"""
    s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
    s.connect(("titan.picoctf.net", 49655))
    
    # Receive initial menu
    response = s.recv(4096).decode()
    
    # Send 'E' for encrypt
    s.send(b'E\n')
    time.sleep(0.1)
    
    # Receive prompt for text
    response = s.recv(4096).decode()
    
    # Send our message
    s.send(message.encode() + b'\n')
    
    # Get response with ciphertext
    response = s.recv(4096).decode()
    print(response)
    s.close()
    
    # Parse: "ciphertext (m ^ e mod n) 135217686007..."
    for line in response.split('\n'):
        if 'ciphertext' in line and 'mod n' in line:
            ciphertext = int(line.split('mod n)')[-1].strip())
            return ciphertext
    return None

# Step 1: Get three ciphertexts
print("=== Getting ciphertexts ===\n")

m1_str = "A"
m1 = bytes_to_long(m1_str.encode())
c1 = interact_oracle(m1_str)
print(f"m1={m1}, c1={c1}\n")

m2_str = "B"
m2 = bytes_to_long(m2_str.encode())
c2 = interact_oracle(m2_str)
print(f"m2={m2}, c2={c2}\n")

m3_str = "C"
m3 = bytes_to_long(m3_str.encode())
c3 = interact_oracle(m3_str)
print(f"m3={m3}, c3={c3}\n")

# Step 2: Find e and n
print("=== Finding e and n ===\n")

common_e = [3, 5, 7, 11, 13, 17, 257, 65537]

found = False
for e in common_e:
    diff1 = pow(m1, e) - c1
    diff2 = pow(m2, e) - c2
    diff3 = pow(m3, e) - c3
    
    if diff1 > 0 and diff2 > 0 and diff3 > 0:
        n = GCD(GCD(diff1, diff2), diff3)
        
        # Verify
        if pow(m1, e, n) == c1 and pow(m2, e, n) == c2 and pow(m3, e, n) == c3:
            print(f"✓ Found: e={e}")
            print(f"✓ Found: n={n}\n")
            found = True
            break

if not found:
    print("Could not find e and n!")
    exit()

# Step 3: Perform CCA attack on the password
print("=== Performing CCA Attack ===\n")

c_password = 2575135950983117315234568522857995277662113128076071837763492069763989760018604733813265929772245292223046288098298720343542517375538185662305577375746934

# Transform the ciphertext
multiplier = 2
c_prime = (c_password * pow(multiplier, e, n)) % n

print(f"Original ciphertext: {c_password}")
print(f"Transformed ciphertext: {c_prime}\n")

# Send transformed ciphertext to oracle for decryption
print("Sending transformed ciphertext to oracle...\n")

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
s.connect(("titan.picoctf.net", 49655))

# Menu
s.recv(4096)

# Choose decrypt
s.send(b'D\n')
time.sleep(0.1)

# Receive decrypt prompt
response = s.recv(4096).decode()
print(response)

# Send the transformed ciphertext
s.send(str(c_prime).encode() + b'\n')

# Get decrypted result
response = s.recv(4096).decode()
print(response)
s.close()

# Parse the decrypted value (m_prime)
for line in response.split('\n'):
    if 'cleartext' in line.lower() or 'decrypted' in line.lower():
        # Extract the number
        parts = line.split(':')
        if len(parts) > 1:
            m_prime = int(parts[-1].strip(), 16)  # Might be in hex
            break
else:
    # Try to find any large number in response
    import re
    numbers = re.findall(r'\d+', response)
    if numbers:
        m_prime = int(numbers[-1])

print(f"\nm_prime (decrypted transformed): {m_prime}")

# Step 4: Recover original password
m = (m_prime * inverse(multiplier, n)) % n
password = long_to_bytes(m)

print(f"\n🎉 Recovered password: {password}")
print(f"   As string: {password.decode()}")

```

- Mathematical exploit using property of RSA (multiplying cipher texts is same as multiplying their plaintexts and the e^mod n)
- So we use a random multipler from our side lets say 2 i used because most basic . So we decrypt this new ciphertext and get our password
- But decrypted output : 2* password
- Dividing by our multipler (2 in this case) gave us our pass 
- Decrypting secret.enc using openssl <given in picoCTF hints>
## Flag:

```
picoCTF{su((3ss_(r@ck1ng_r3@_24bcbc66}
```

## Concepts learnt:

- RSA algorithm full working
 - Plain text attack , Properties of RSA
- CPA , CCA vulnerability exploitation
## Notes:

- I tried to look for other methods<manual> to find e and n but since they were so big a script was the perfect way
- Had problem at the "multiplier" part. Understood the logic till finding e and n

## Resources:

- Claude.


***

# 2. Custom Encryption

> Can you get sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag.
## Solution:

- Checking the source code
- 2 main types of encryption
 - "encrypt" and dynamic_XOR

- Decryption method
 1. undo "encrypt"
 2. undo "dynamic_XOR"
 3. Reverse the plaintext

- We have to 2 type of keys : "shared_key" and XOR key "trudeau"
- We need to calculate the value of shared key using given data
- Calculation of Shared key
 - We calculate individual keys first
 ```
 Calculate v and u as per the given info in the code
 pow(v, a, p) and pow(u, b, p) respectively using given data.
 
 ```
- We get shared_key=6 as both keys=6
- In "encrypt"
 : (ord(char) * key * 311)
 To decrypt we divide the elements of "cipher" by key*311 
- End up with a semi cypher
- Now applying XOR with key repeating
- Revering the plaintext and getting the key

## Flag:

```
picoCTF{custom_d2cr0pt6d_49fbee5b}
```

## Concepts learnt:

- Understanding Diffie-hellman system
- Understand "keys" in the diffie hellman system and the secret values a and b
- XOR encryption and reversing text
## Notes:

- initially calculated the key wrong and cried quite a bit.<didnt , sike!>
- Dcode gave me some trouble <they want me to unblock trackers and ads . Dawg i use GX >
## Resources:

- Claude.


***