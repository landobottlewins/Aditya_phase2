# IRONCTF 2024: Uncrackable Zip
> description: "I forgot to ask my friend for the password to access the secret webpage, and now he's unavailable. I've tried guessing the password, but it's been unsuccessful. My friend is very security-conscious, so he wouldn't use an easy-to-guess password. He's also an Emmet enthusiast who relies heavily on code generation shortcuts. Can you help me figure out how to access the secret webpage?<br><b>Author</b>: `AbdulHaq`"
>
> files: handout/website.zip

## solution:

<img width="1095" height="860" alt="image" src="https://github.com/user-attachments/assets/c9c6f761-ea65-49f4-bb56-3ff1ed07515a" />

- inspecting the zip file using `7z l -slt` we can see the encryption method is ZipCrypto
- ZipCrypto is vulnerable to a Known Plaintext Attack (KPA) if we know at least 12 continuous bytes of the uncompressed data inside.
- The description emphasizes "Emmet" and "shortcuts." In web development, Emmet allows us to type ! or html:5 and hit tab to generate a standard HTML5
- create plain.txt and put html header in it `<!DOCTYPE html>`
- now we can use `bkcrack` to crack the zip using the command

```
PS C:\Users\Aditya\Downloads\bkcrack-1.8.1-win64\bkcrack-1.8.1-win64> .\bkcrack.exe -C website.zip -c index.html -p plain.txt
bkcrack 1.8.1 - 2025-10-25
[22:43:36] Z reduction using 15 bytes of known plaintext
100.0 % (15 / 15)
[22:43:36] Attack on 498946 Z values at index 6
Keys: a18ba181 a00857dd d953d80f
65.1 % (324924 / 498946)
Found a solution. Stopping.
You may resume the attack with the option: --continue-attack 324924
[22:46:31] Keys
a18ba181 a00857dd d953d80f
```

```
PS C:\Users\Aditya\Downloads\bkcrack-1.8.1-win64\bkcrack-1.8.1-win64> .\bkcrack.exe -C website.zip -c index.html -k a18ba181 a00857dd d953d80f -d index.html
bkcrack 1.8.1 - 2025-10-25
[23:19:01] Writing deciphered data index.html
Wrote deciphered data (not compressed).
```

## Flag
```
ironCTF{Wh0_us35_Z1pCrypt0_wh3n_kn0wn_PlA1nt3xt_a7t4cks_ex1s7?}
```
