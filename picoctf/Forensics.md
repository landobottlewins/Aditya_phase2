# trivial flag transfer protocol
> Figure out how they moved the flag.

## Solution:

- The file is a `.pcapng` which is associated to wireshark 
- Since this challenge is based on tftp we can go straight to export tftp objects
<img width="615" height="630" alt="image" src="https://github.com/user-attachments/assets/6565a600-4a97-465b-846b-2bcc258d1da9" />


- 6 items get exported which are
```
instructions.txt  picture1.bmp  picture2.bmp  picture3.bmp  plan  program.deb
```

- `instructions.txt` reads the following and appears to be encoded from something
```
GSGCQBRFAGRAPELCGBHEGENSSVPFBJRZHFGQVFTHVFRBHESYNTGENAFSRE.SVTHERBHGNJNLGBUVQRGURSYNTNAQVJVYYPURPXONPXSBEGURCYNA
```
- putting this txt file in online identifier (https://www.boxentriq.com/code-breaking/cipher-identifier#caesar-cipher) 
<img width="1319" height="837" alt="image" src="https://github.com/user-attachments/assets/6732c312-538c-421f-956c-511e049120a1" />


- similarly for the `plan` file
  <img width="1068" height="816" alt="image" src="https://github.com/user-attachments/assets/29d4b71f-162c-4967-a321-84c4666dc3db" />


- exploring the `.deb` using 7zip reveals that `steghide` was probably used to hide the flag
- using steghide command  `steghide extract -sf picture1.bmp`
- now it asks for a passphrase which can be found on decoding the `plan` file i.e. "DUEDILIGENCE"
- trying for all three pictures reveals the flag with the `picture3`
```
 aditya@Dell-Inspiron:~/tftp$ steghide extract -sf picture1.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
aditya@Dell-Inspiron:~/tftp$ steghide extract -sf picture2.bmp
Enter passphrase:
steghide: could not extract any data with that passphrase!
aditya@Dell-Inspiron:~/tftp$ steghide extract -sf picture3.bmp
Enter passphrase:
wrote extracted data to "flag.txt".
```
reading the flag.txt reveals the flag `picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}`

## Flag:

```
picoCTF{h1dd3n_1n_pLa1n_51GHT_18375919}
```

## Concepts learnt:

- `.pcapng` files are used to store metadata, such as information about different capture interfaces, drop counters, and DNS records
- went through basics of steghide 

## Notes:

-  `picture2.bmp` looks suspiciously large compared to others since its nearly 35mb (apparently it isn't giving flag)
-  `due diligence` was a tough nut to crack because at first I thought it was an incorrect tangent but realised that I should try the combinations `duediligence` and `DUE DILIGENCE` and `DUEDILIGENCE`
-  for some reason I couldn't get all the files (only instructions.txt) on my windows install of wireshark but on linux all 6 showed up

## Resources:

- (https://pcapng.com/)
- (https://www.boxentriq.com/code-breaking/cipher-identifier)


***


# m00walk
> Decode this message from the moon.


## Solution:

- the hint tells us to use the sstv utility (https://github.com/colaclanth/sstv)
- installing the utility and using it
```
aditya@Dell-Inspiron:~$ git clone https://github.com/colaclanth/sstv.git 
aditya@Dell-Inspiron:~$ cd sstv
aditya@Dell-Inspiron:~/sstv$ sudo python3 setup.py install
aditya@Dell-Inspiron:~/sstv$ cd ~

aditya@Dell-Inspiron:~$ sstv -d message.wav -o result.png
[sstv] Searching for calibration header... Found!
[sstv] Detected SSTV mode Scottie 1
[sstv] Decoding image...   [######################################################################################] 100%
[sstv] Drawing image data...
[sstv] ...Done!
``` 
<img width="320" height="256" alt="image" src="https://github.com/user-attachments/assets/90278648-e5e1-4c9a-993c-9c49ed204b0d" />
rotate the image to see the flag `picoCTF{beep_boop_im_in_Space}`


## Flag:

```
picoCTF{beep_boop_im_in_Space}
```

## Concepts learnt:

- Audio can be encoded in a format that can be used to send data


## Notes:

- .pcapng files are used to store metadata, such as information about different capture interfaces, drop counters, and DNS records

## Resources:

- (https://github.com/colaclanth/sstv)
- (https://en.wikipedia.org/wiki/Apollo_11_missing_tapes)



***
