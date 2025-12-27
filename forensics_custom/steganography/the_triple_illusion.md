# The Triple Illusion
> Some things are hidden in plain sight. Use your knowledge of forensics and crypto to solve the challenge
> "The_Triple_Illusion/images/hibiscus.png",
    "The_Triple_Illusion/images/roses.png",
    "The_Triple_Illusion/datavsmetadata.png"


## Solution:
- checking binwalk
<img width="2222" height="916" alt="image" src="https://github.com/user-attachments/assets/51dd0ada-451f-4a72-8ef1-acdbe5ec79c0" />

- checking strings
<img width="2621" height="938" alt="image" src="https://github.com/user-attachments/assets/12753396-5f85-43f8-91f2-27c13eca1922" />

hidden in plain sight could be steganography

- checking zsteg
<img width="2863" height="1149" alt="image" src="https://github.com/user-attachments/assets/91511cd2-1561-43e3-9183-88550070fa41" />

- this looks like a shift cipher likely to be Vigenere so i confirmed it 
<img width="1426" height="875" alt="image" src="https://github.com/user-attachments/assets/f5f38d46-fd02-42d0-896b-6067c4d988c9" />

- now we have the another which from the metadata seems is a encrypted using XOR
- We can write a python script to solve the encryption

```python
from itertools import cycle

def solve_challenge():
    key = "csawctf{heres_anew_key_decrypt_the_secretto_reveal_flag}"

    ciphertext = [
        0, 0, 0, 0, 0, 0, 0, 0, 15, 23, 23, 4, 7, 0, 22, 23, 29, 25, 0, 18,
        10, 12, 0, 7, 23, 2, 17, 18, 21, 16, 0, 0, 0, 0, 0, 28, 7, 16,
        17, 16, 6, 17, 11, 0, 1, 0, 21, 23, 4, 24, 0, 0, 0, 0, 0, 0
    ]

    plaintext = [
        chr(c ^ ord(k))
        for c, k in zip(ciphertext, cycle(key))
    ]

    print("".join(plaintext))

if __name__ == "__main__":
    solve_challenge()
```
We finally get the flag


## Flag:

```
csawctf{great_wyxn_you_cracked_the_obscured_secret_flag}
```
