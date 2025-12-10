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
