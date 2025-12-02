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







