# WaniCTF 2024: tiny10px

## Solution:

- opening the image it seems that it is too small for the file size
- checking the usual tools reveals nothing which implies that it must be related to editing the hex
- Opened the file in a hex editor and searched for the Start of Frame (SOF) marker.
- Identified the segment starting `FF C0`
- After FF C0, skip 3 bytes. The next two bytes are the height, and the two after that are the width.
<img width="1124" height="374" alt="image" src="https://github.com/user-attachments/assets/20c8b432-6ece-4991-a792-05a13e8b4d0b" />

- `00 0A` means the height is 10px
- setting it to 100px which is `00 64` and `00 64` gives something

![chal_tiny_10px](https://github.com/user-attachments/assets/280e1244-bd88-458c-83b0-5d00e190758d)

- by trial and error i found that `00 99` and `00 99` gives the clear image of the flag
 
![chal_tiny_10px](https://github.com/user-attachments/assets/ed91bbd0-04bb-458b-8f3b-b67508c5d1b6)

## Flag:
```
FLAG{b1g_en0ugh}
```
