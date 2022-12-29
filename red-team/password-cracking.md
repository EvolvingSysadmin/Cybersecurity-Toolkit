---
permalink: /password-cracking.html
title: Password Cracking
description: 
---
<head>
<link href="css/cyber.css" rel="stylesheet">
</head>

[Toolkit Homepage](../README.md)

### Password Cracking

Windows SAM structure: MD5

https://www.hackingarticles.in/credential-dumping-sam/

#### Hashcat

Password Cracking Using Kali using Hashcat

echo 'hash' > user.hash

hashcat -a 0 user.hash rockyou.txt -m = 1800
hashcat -a 0 user.hash rockyou.txt -m = 1800 --show

Breakdown:
-a 0: designates dictionary attack
user.hash: file contains hash being cracked
rockyou.txt: wordlist
- m 1800: type 6 password hash

#### John

