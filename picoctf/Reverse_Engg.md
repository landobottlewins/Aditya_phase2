# GDB baby step 1
> Can you figure out what is in the eax register at the end of the main function? Put your answer in the picoCTF flag format: picoCTF{n} where n is the contents of the eax register in the decimal number base. If the answer was 0x11 your flag would be picoCTF{17}.
Disassemble this.

## Solution:

- The first thing, I ran `exiftool debugger0_a` to read the metadata of the file which gave the output 
```
ExifTool Version Number         : 12.40
File Name                       : debugger0_a
Directory                       : .
File Size                       : 16 KiB
File Modification Date/Time     : 2025:10:22 19:07:31+05:30
File Access Date/Time           : 2025:10:22 19:08:59+05:30
File Inode Change Date/Time     : 2025:10:22 19:08:13+05:30
File Permissions                : -rw-r--r--
File Type                       : ELF shared library
File Type Extension             : so
MIME Type                       : application/octet-stream
CPU Architecture                : 64 bit
CPU Byte Order                  : Little endian
Object File Type                : Shared object file
CPU Type                        : AMD x86-64

```

- Here the file is a `.so` which can be "disassembled" using `gdb` as mentioned in the hint 
- Next, I ran the `gdb ./debugger0_a` command to open the file in gdb followed by `disassemble main` to disassemble the main function   
```
Dump of assembler code for function main:
   0x0000000000001129 <+0>:     endbr64
   0x000000000000112d <+4>:     push   %rbp
   0x000000000000112e <+5>:     mov    %rsp,%rbp
   0x0000000000001131 <+8>:     mov    %edi,-0x4(%rbp)
   0x0000000000001134 <+11>:    mov    %rsi,-0x10(%rbp)
   0x0000000000001138 <+15>:    mov    $0x86342,%eax
   0x000000000000113d <+20>:    pop    %rbp
   0x000000000000113e <+21>:    ret
End of assembler dump.
(gdb)
```

- Now we can read the hex value associated with `eax` which is `0x86342`
- Converting hex to decimal we get `549698` which gives the flag `picoCTF{549698}`

## Flag:

```
picoCTF{549698}
```

## Concepts learnt:

- What is gdb and how we can use it to disassemble a file
- gdb is a debugging program to examine problems for C and C++
- disassembler translates machine language into assembly language
- `gdb ./debugger0_a` and `disassemble main` are the basic commands


## Resources:

- (https://visualgdb.com/gdbreference/commands/disassemble)
- (https://www.tutorialspoint.com/gnu_debugger/what_is_gdb.htm)

***



# ARMssembly 1
For what argument does this program print `win` with variables 83, 0 and 3? File: chall_1.S Flag format: picoCTF{XXXXXXXX} -> (hex, lowercase, no 0x, and 32 bits. ex. 5614267 would be picoCTF{0055aabb})

## Solution:




## Flag:

```
picoCTF{0000001b}
```

## Concepts learnt:

- A `.S` file is a source code file written in assembly language

## Notes:



## Resources:

- (https://stackoverflow.com/questions/7190050/how-do-i-compile-the-asm-generated-by-gcc)
 - (https://www.youtube.com/watch?v=1d-6Hv1c39c)
 - (https://youtube.com/playlist?list=PLMB3ddm5Yvh3gf_iev78YP5EPzkA3nPdL&si=HXrDUtjPeSTMZQfE)

***

# Vault door 3
