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
- The challenge uses a file with `.S` extension which stores the source code of a program in assembly
- We can analyse the code to understand what it does
- the required "win" condtion is mentioned in .LC0 which means it must be mentioned somewhere in the code where it is called


reading the `func`
```
	mov	w0, 83
	str	w0, [sp, 16]
	str	wzr, [sp, 20]
	mov	w0, 3
	str	w0, [sp, 24]
```
gives values and memory location 
- 83 is at sp 16
- 0 is at sp 20
- 3 is at sp 24



```
	ldr	w0, [sp, 20]
	ldr	w1, [sp, 16]
	lsl	w0, w1, w0
	str	w0, [sp, 28]
```
- w0 = 0
- w1 = 833

`lsl` is logical shift left
- w0 = w1 << w0
- 83 << 0 = 83
now it stores 83 at sp 28 



```
	ldr	w1, [sp, 28]
	ldr	w0, [sp, 24]
	sdiv	w0, w1, w0
	str	w0, [sp, 28]
```
- w0 at sp 24
- sdiv will perform integer division
- 83/3 = 27 and stored in sp 28 now




```	
    ldr	w1, [sp, 28]
	ldr	w0, [sp, 12]
	sub	w0, w1, w0
	str	w0, [sp, 28]
```
- w1 = 27
- w0 is set to a new memory location sp 12 which means a new variable (assume a)
- w0 = 27 - a 
- and now 27 - a is stored in sp 28




```
	ldr	w0, [sp, 28]
	add	sp, sp, 32
	ret
```
- w0 = 27 - a




Now, we look for where LC0 is called to get the win condition. It is mentioned towards the end of the `main`
```
adrp	x0, .LC0
	add	x0, x0, :lo12:.LC0
	bl	puts
	b	.L6
``` 
it indicates that the function must return 0 to get the win condition 

- hence 27 - a = 0 and a is 27.
- So the argument to be passed with the program must be 27 to print win.
- hex(27) = 0x1b


thus the required flag is picoCTF{0000001b}





## Flag:

```
picoCTF{0000001b}
```

## Concepts learnt:

- A `.S` file is a source code file written in assembly language
- Basics of comprehending assembly code for arm64
- Assembly code for different architechtures is slightly different

## Notes:
- I tried to run the program through arm emulation on x64 on qemu and gcc (idk i couldnt figure out exactly)


## Resources:

- (https://stackoverflow.com/questions/7190050/how-do-i-compile-the-asm-generated-by-gcc)
 - (https://www.youtube.com/watch?v=1d-6Hv1c39c)
 - (https://youtube.com/playlist?list=PLMB3ddm5Yvh3gf_iev78YP5EPzkA3nPdL&si=HXrDUtjPeSTMZQfE)
- (https://diveintosystems.org/book/C9-ARM64/common.html)
***

# Vault door 3
