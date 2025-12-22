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
