# CSAW 2024 Qualifiers: I Like My Camera RAW
> "description": "Seems like medium rare just isn't my taste.",
>
> "files": ["DSCF3911.RAF", "message.txt.gpg", "secret.png"]

## Solution

- Using zsteg to find data from the image
```
┌──(kali㉿kali)-[~/Desktop]
└─$ zsteg secret.png
imagedata           .. text: "===,-,\t\t\t"
b1,bgr,lsb,xy       .. text: "m a big, bold ad, a flashy show,\nAbove a brick wall, red as a pale tomato.\nSpillin"
b3,g,lsb,xy         .. file: GLS_BINARY_MSB_FIRST
b4,g,msb,xy         .. file: GTA audio index data (SDT)

```

```
┌──(kali㉿kali)-[~/Desktop]
└─$ zsteg -E "b1,bgr,lsb,xy" secret.png > output.txt
```

<img width="849" height="600" alt="image" src="https://github.com/user-attachments/assets/7c91d13b-6d08-4001-a55d-a6f3400f0c6e" />

- the metadata of the `DSCF3911.RAF` file gives us the coordinates of a location
<img width="833" height="217" alt="image" src="https://github.com/user-attachments/assets/df41513a-b9ef-4ccc-8ea3-422939aa4da5" />

```
40.7043749, -73.9903311
```

checking on google maps
<img width="1493" height="1010" alt="image" src="https://github.com/user-attachments/assets/5f245265-6318-49e5-bbc0-bf7993915e49" />

- There is a phone number on the "60 Water" billboard
```
7182223300
```

- Decrypt the message.txt.gpg using the number as password
```
                                                                                                                                                                                                                                            
┌──(kali㉿kali)-[~/Desktop]
└─$ gpg -d message.txt.gpg
gpg: keybox '/home/kali/.gnupg/pubring.kbx' created
gpg: AES256.CFB encrypted data
gpg: encrypted with 1 passphrase
csawctf{1_kN0w_Y0U_l1k3_1T_R4W}
```

## Flag:
```
csawctf{1_kN0w_Y0U_l1k3_1T_R4W}
```

