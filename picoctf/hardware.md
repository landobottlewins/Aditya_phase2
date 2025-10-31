# IQ-Test
> Let your input x = 30478191278.
wrap your answer with nite{ } for the flag.
As an example, entering x = 34359738368 gives (y0, ..., y11), so the flag would be nite{010000000011}.

## Solution
Here is a complex mesh of logic gates 
<img width="1225" height="2176" alt="iqtest (2)" src="https://github.com/user-attachments/assets/df410887-7a98-4c6e-b3be-f4531a3f5824" />

- the first logical step is to convert the x = 30478191278 from decimal to binary since the logic gates will only take 1 and 0 and theres 36 of them
- `30478191278` in binary is `011100011000101001000100101010101110` which is 36 digits as expected
- I believe the only method to this madness is to manually check the output of every gate and meticulously calulate the output


Hence the final value is `100010011000` this the flag is `nite{100010011000}`

## Flag:

```
nite{100010011000}
```

## Concepts learnt: 
- Revised logic gates thoroughly 


## Notes:
- on converting to binary I realised that its only 35 digits and thought the question was incorrect
- then I realised that the first digit is simply "0"



***

# I like Logic

> i like logic and i like files, apparently, they have something in common, what should my next step be.
challenge.sal

## Solution:

- The .sal extension and the the description reveals that the file is associated with "Saleae's Logic 2".
- So I installed the app and opened the file to find some signal
- Only channel 3 has the signal
- I looked up what the sal utilities did and analyzed it
<img width="663" height="746" alt="image" src="https://github.com/user-attachments/assets/1bb4ea8e-412b-44f4-878e-81709ded96a8" />

- Then it shows a text output which gives away the flag
<img width="1919" height="1018" alt="image" src="https://github.com/user-attachments/assets/48dff88d-2f5a-48a6-9c78-c2d588ac0e52" />



## Flag:

```
FCSC{b1dee4eeadf6c4e60aeb142b0b486344e64b12b40d1046de95c89ba5e23a9925}
```

## Concepts learnt:

- Data can be encoded in signals and interpreted using some specialized apps

## Notes:

- at first it seemed to be a zip file renamed for confusion
- then i found binary files but they didnt make any sense
- I looked up what the .sal was and then found the software

## Resources:

- (https://www.youtube.com/watch?v=Ak9R4yxQPhs)



***


# Bare Metal Alchemist
> my friend recommended me this anime but i think i've heard a wrong name.
HINT: XOR

## Solution:

- The attached file is a .elf which is associated with linux executable program
```
aditya@Dell-Inspiron:~$ file firmware.elf
firmware.elf: ELF 32-bit LSB executable, Atmel AVR 8-bit, version 1 (SYSV), statically linked, with debug_info, not stripped
```
- We probably need to disassemble the whole thing to find out
- So I installed ghidra and opened the main function to look for XOR `^`

```
  R25R24._0_1_ = DAT_mem_0029;
R25R24._0_1_ = (byte)R25R24 ^ (byte)R25R24 * '\x02';
if (((byte)R25R24 & 4) == 0) {
    auStack_7 = (undefined1  [3])0x141;
    z1();
}
```
- this reads a byte from `DAT_mem_0029`
- then XOR the value with itself shifted left by one bit.

```
R25R24._0_1_ = *(byte *)(uint3)R15R14;
if ((byte)R25R24 == 0) break;
Z._1_1_ = (undefined1)(R15R14 >> 8);
Z._0_1_ = (byte)R25R24 ^ R11;
if ((byte)R25R24 == 0xa5) break;
```
`R15R14` is initialized to `0x68`, so it’s reading from program memory address `0x68`. Then it loads a byte from program

so basically it does this
```
data ^= 0xa5
```

thus the XOR key is `0xa5` which is `A5`


- Now taking a look at disassembly (before the function starts)

<img width="1919" height="1019" alt="image" src="https://github.com/user-attachments/assets/cf135a36-6fef-4ce4-8b7d-82d853627798" />


- while the listing itself doesn't really give anything the hex bytes do
- pasting the hex bytes into XOR decrypt tool gives away the flag `TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}`
  <img width="1919" height="905" alt="image" src="https://github.com/user-attachments/assets/21a7065b-5897-418b-ad7b-0c73e887825e" />


## Flag:

```
TFCCTF{Th1s_1s_som3_s1mpl3_4rdu1no_f1rmw4re}
```


## Concepts learnt:
- This challenge required quite some concepts to be understood in disassembly and reverse engineering.
- Learning to use ghidra was crucial for this
- Hex bytes can also be encrypted
- How XOR is implemented

  
## Notes:
- My first thought was "no way I have to do disassembly for this" and it turns out I couldnt be further off track.
- I checked `file` `string` `exiftool` `binwalk` then looked up what .elf was.
- I tried executing the program but it wont run since its meant for arduino... So yes disassembly it is
- Using gdb for it felt insufficient for this so I installed ghidra and learnt how to use it
- I didnt understand anything but then got the XOR hint so I checked the main function pseudocode for `^` which is used in C for XOR operation


## Resources: 
- (https://www.varonis.com/blog/how-to-use-ghidra)
- (https://github.com/NationalSecurityAgency/ghidra)
- (https://www.geeksforgeeks.org/c/bitwise-operators-in-c-cpp/)
- (https://en.wikipedia.org/wiki/Executable_and_Linkable_Format)
- (https://md5decrypt.net/en/Xor/)
