# RSA oracle
> Can you abuse the oracle?
An attacker was able to intercept communications between a bank and a fintech company. They managed to get the message (ciphertext) and the password that was used to encrypt the message.
After some intensive reconassainance they found out that the bank has an oracle that was used to encrypt the password and can be found here nc titan.picoctf.net 57280. Decrypt the password and use it to decrypt the message. The oracle can decrypt anything except the password.

## Solution:

- We have 2 .enc files. 
- The `password.enc` file has been encrypted using the oracle at ` nc titan.picoctf.net 51988`
- The `secret.enc` can be decrypted using openssl
- The oracle can both encrypt and decrypt anything but the password


- we can exploit this using the homomorphic property 
```
what should we do for you?
E --> encrypt D --> decrypt.
e
enter text to encrypt (encoded length must be less than keysize): 2
2

encoded cleartext as Hex m: 32

ciphertext (m ^ e mod n) 4707619883686427763240856106433203231481313994680729548861877810439954027216515481620077982254465432294427487895036699854948548980054737181231034760249505
```
- now we have (32^e mod n) and we can multiply it by (n^e mod n) to get ((32n)^e mod n) {here n is the password in hex}
- doing that using python and decoding it from the oracle
```
a = 4707619883686427763240856106433203231481313994680729548861877810439954027216515481620077982254465432294427487895036699854948548980054737181231034760249505
b = 1634668422544022562287275254811184478161245548888973650857381112077711852144181630709254123963471597994127621183174673720047559236204808750789430675058597

print(a*b)
```

```
what should we do for you?
E --> encrypt D --> decrypt.
d
Enter text to decrypt: 7695397569202567845951534995458665507673343348096363340982737850957127633396299903805483751028603032381992074198003879773911743796997290925102562656506047424891066644072050561546038487523471370829681341678338287909825421864390772820326423850644272250884967018175331985857554152142511343509574844412215244485
decrypted ciphertext as hex (c ^ d mod n): a332c646dba
decrypted ciphertext:
3,dmº

```


since we cant directly divide hex numbers like decimal (we cant directly do 32n/32 = n)
- converting the decrypted hex ciphertext 32n = `a332c646dba` to decimal gives `11214904389050`
- hex `32` gives decimal `50`


- now we can divide both values to get the n (decimal) `224298087781`
- converting this back to hex `3439353565`
- now convert hex to text string `4955e` THIS IS THE PASSWORD!


- Using openssl to decrypt the secret.enc
```
aditya@Dell-Inspiron:~$ openssl enc -aes-256-cbc -d -in secret.enc
enter AES-256-CBC decryption password:
*** WARNING : deprecated key derivation used.
Using -iter or -pbkdf2 would be better.
picoCTF{su((3ss_(r@ck1ng_r3@_4955eb5d}
```


## Flag:

```
picoCTF{su((3ss_(r@ck1ng_r3@_4955eb5d}
```

## Concepts learnt:

- What is RSA encryption
- Expoliting RSA using maths (homomorphic property)


## Notes:

The RSA homomorphic property is a multiplicative one, meaning that if you multiply two encrypted messages, the result is the same as encrypting the product of the original messages. E(a).E(b)=E(a.b)

## Resources:

- (https://www.geeksforgeeks.org/computer-networks/rsa-algorithm-cryptography/)
- (https://youtu.be/hm8s6FAc4pg?si=gfyApKc8bXvFDm1Y)
- (https://www.rapidtables.com/convert/number/hex-to-decimal.html)


***


# Custom encryption
> Can you get sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag.


## Solution

from the the given code we can analyze and write code to find the encryption key
```
def generator(g, x, p):
    return pow(g, x) % p
p = 97 
g = 31

a = 94
b = 21
cipher = [131553, 993956, 964722, 1359381, 43851, 1169360, 950105, 321574, 1081658, 613914, 0, 1213211, 306957, 73085, 993956, 0, 321574, 1257062, 14617, 906254, 350808, 394659, 87702, 87702, 248489, 87702, 380042, 745467, 467744, 716233, 380042, 102319, 175404, 248489]

v = generator(g, b, p)
key = generator(v, a, p)

print(key)
```

which gives the output
```
47
```

reversing the encryption modifying the encryption code 
```
decipher = ""
for c in cipher:
    decipher(c // key // 311)
print(decipher)
```

reversing the xor encryption
```
def dynamic_xor_decrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    k = ""
    for i, char in enumerate(plaintext[::-1]):
        key_char = text_key[i % key_length]
        k += key_char
        encrypted_char = chr(ord(char) ^ ord(key_char))
        cipher_text += encrypted_char
    print(k)
    return cipher_text
```

compiling all this together and printing the flag with `print(dynamic_xor_decrypt(decipher, "aedurtu"))`

this gives us the flag `picoCTF{custom_d2cr0pt6d_8b41f976}`

## Flag:

```
picoCTF{custom_d2cr0pt6d_8b41f976}
```

## Concepts learnt:

- Got to extensively work on python 


## Resources:

- (https://www.geeksforgeeks.org/dsa/xor-cipher/)


***


# MiniRSA
> Let's decrypt this: ciphertext? Something seems a bit small.


## Solution

- We have the N, e and the ciphertext.
- We just need to find the original message
- I looked up if there were any tools for it and there is one (https://www.dcode.fr/rsa-cipher)

<img width="1357" height="820" alt="image" src="https://github.com/user-attachments/assets/ebd529c3-22a5-46af-9c14-c3d0cdcc035d" />
this gives away the flag easily

## Flag:

```
picoCTF{n33d_a_lArg3r_e_606ce004}
```

## Notes:

- I thought that if n>m^e then (m^e) mod n = m^e


## Resources:

- (https://www.dcode.fr/rsa-cipher)


***






