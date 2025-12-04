# Hide and seek
> Description: Sakamoto’s at it again with a game of hide and seek, but this time, it’s not with Shin or his daughter. An old friend hid some secret data in this image. Can you find it before the others do? Hint: Even in retirement, Sakamoto never loses at hide and seek. Maybe stegseek can help you keep up.


## Solution:

- As the hint clearly gives it away, we need to use stegseek for this challenge.
- So I downloaded stegseek from here https://github.com/RickdeJager/stegseek?tab=readme-ov-file
- now simply run the command stegseek `sakamoto.jpg`
  
```                                                                  
┌──(kali㉿kali)-[~/Desktop]
└─$ stegseek sakamoto.jpg
StegSeek 0.6 - https://github.com/RickdeJager/StegSeek

[i] Found passphrase: "iloveyou1"
[i] Original filename: "flag.txt".
[i] Extracting to "sakamoto.jpg.out".
```

- we can read the flag in the `sakamoto.jpg.out`

## Flag:

```
nite{h1d3_4nd_s33k_but_w1th_st3g_sdfu9s8}
```

## Concepts Learnt:
- Stegseek is the tool you would use to try and guess the password and reveal the hidden information
- It performs a dictionary attack or brute-force attack, using a wordlist to try and unlock the hidden content.


## Notes:
- The wordlists weren't installed by default so I had to run `sudo apt install wordlists` and unzip it with `sudo gzip -d /usr/share/wordlists/rockyou.txt.gz`

## Resources:

- (https://github.com/RickdeJager/stegseek?tab=readme-ov-file)

***

# Nutrela Chunks
> One of my favorite foods is soya chunks. But as I was enjoying some Nutrela today, I noticed a few chunks weren’t quite right. Seems like something’s off with their structure. Could you help me fix these broken chunks so I can enjoy my meal again?

## Solution:
- The description leads that some chunks were broken, i.e. some of the hex bytes of the file were incorrect.
- The file is a .png which has corrupted headers
- taking a look at the hex
<img width="864" height="491" alt="image" src="https://github.com/user-attachments/assets/f5d65367-e185-4c84-9bbb-2d3120c9943e" />

- the first obvious error is the `PNG`, `IHDR` and `IDAT` are in lowercase
- fixing it by converting to uppercase
<img width="956" height="514" alt="image" src="https://github.com/user-attachments/assets/20c8bf7a-ab11-4145-aa28-a262ac1a71c4" />

- This fixed the image and we can read the flag from it


## Flag:
```
nite{n0w_y0u_kn0w_ab0ut_PNG_chunk5}
```


## Concepts learnt:

- first 8 bytes hold the png header
- what are critical chunks in a png


## Resources:

- (https://medium.com/@0xwan/png-structure-for-beginner-8363ce2a9f73)
- (https://medium.com/@mihiredu11/bupcsc-ctf-83b175f697da)

***

# RAR of the Abyss
> Two philosophers peer into the networked abyss and swap a secret. Use the secret to decrypt the Abyss’ RAwR and pull your flag from the void.


## Solution:

- As the description suggests we need to look for a RAR file.
- going through the file in wireshark, we see some conversations 

<img width="1914" height="1025" alt="image" src="https://github.com/user-attachments/assets/50c98f5c-cd9a-4110-917b-dba8372f1a6a" />

- when we follow the TCP stream, we see many other streams with a full conversation and a RAR file with its password later
```
Camus: One must imagine Sisyphus happy but are we happy ?

Nietzsche: You will be happy after reading my latest work

Rar!........!.....dE7.'.a.U...u..gC...f.C..._R...wOdp..&...ka*.sG.......qxr..]._....o......[*...V.DP..k.I..T.w~............1...:o:....KOd.q..9...Z.....|..aY.R...[..q8......}...A.!\..E..7L!.|.../.........)).{ja.....t....=...........A...j%..X..........y...k...q2....Ku..v.g...\y{.....R.J.

Camus: whats the password ?

Nietzsche: b3y0ndG00dand3vil

Camus: thanks
```

<img width="1292" height="912" alt="image" src="https://github.com/user-attachments/assets/e3ced2ee-1d4d-4fa4-a822-78739dfc507f" />

- now we can save the rar and extract it using the password

<img width="1186" height="578" alt="image" src="https://github.com/user-attachments/assets/b8d6bacb-8696-430c-918e-eade57396fa2" />

- Now we can see the flag.txt that is saved in the archive


## Flag:
```
nite{thus_sp0k3_th3_n3tw0rk_f0r3ns1cs_4n4lyst}
```

## Concepts learnt:
- we can read the conversation in a tcp stream and also extract files from it


***


# NineTails
> Description: Looks like I got a little too clever and hid the flag as a password in Firefox, tucked away like one of NineTails’ many tails. Recover the "logins" and the "key4" and let it guide you to the flag. Hint: I named my Ninetails "j4gjesg4", quite a peculiar name isn't it?

## Solution
- Extracting the `.rar` file we find a `.ad1` file inside it.
- The .ad1 file is a disk image file that can be opened using the `FTK imager` tool.
- add the disk image as an evidence item under the file menu
- Now we can see the contents of the disk image
- The description and hint tells us to find the directory where the passwords are stored and view the `j4gjesg4` profile
- So we need to find the `/AppData/Roaming/Mozilla/Firefox/Profiles/j4gjesg4` in one of the users directory
<img width="1409" height="969" alt="image" src="https://github.com/user-attachments/assets/c680a3e8-27ef-45f2-b38a-ce2e8f25f7ac" />

- Now we can decrypt the passwords using this utility https://github.com/unode/firefox_decrypt
- Export the `j4gjesg4.default-release` folder and run the command
```

C:\Users\adt10\Downloads>python firefox_decrypt.py j4gjesg4.default-release
2025-12-04 14:52:01,860 - WARNING - Running with unsupported encoding 'locale': cp1252 - Things are likely to fail from here onwards
2025-12-04 14:52:01,916 - WARNING - profile.ini not found in j4gjesg4.default-release
2025-12-04 14:52:01,916 - WARNING - Continuing and assuming 'j4gjesg4.default-release' is a profile location

Website:   https://www.rehack.xyz
Username: 'warlocksmurf'
Password: 'GCTF{m0zarella'

Website:   https://ctftime.org
Username: 'ilovecheese'
Password: 'CHEEEEEEEEEEEEEEEEEEEEEEEEEESE'

Website:   https://www.reddit.com
Username: 'bluelobster'
Password: '_f1ref0x_'

Website:   https://www.facebook.com
Username: 'flag'
Password: 'SIKE'

Website:   https://warlocksmurf.github.io
Username: 'Man I Love Forensics'
Password: 'p4ssw0rd}'
```
- Now we can simply read the flag from the "Password"

## Flag:
```
GCTF{m0zarella_f1ref0x_p4ssw0rd}
```

## Concepts learnt:

- We can retrive .ad1 disk images using FTK imager
- How to use FTK imager
- Browser passwords `logins` are stored locally and are encrypted with the `key4`

## Resources:

- (https://github.com/unode/firefox_decrypt)
- (https://www.exterro.com/digital-forensics-software/ftk-imager)
