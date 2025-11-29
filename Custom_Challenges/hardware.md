# Formwear
`README.md` 
> this is most definitely going to ring some bells for those who attended the "router hijacking" workshop L0L

## Solution:
- Extracting the `formwear.zip` reveals 3 files `fwu_ver` `hw_ver` and `rootfs`
- using `file` to analyse
```
 aditya@Dell-Inspiron:~$ file fwu_ver
fwu_ver: ASCII text
aditya@Dell-Inspiron:~$ file hw_ver
hw_ver: X1 archive data
aditya@Dell-Inspiron:~$ file rootfs
rootfs: Squashfs filesystem, little endian, version 4.0, zlib compressed, 10936182 bytes, 910 inodes, blocksize: 131072 bytes, created: Sun Oct  1 07:02:43 2023
```

- check the contents with `cat`
```
aditya@Dell-Inspiron:~$ cat fwu_ver
3.0.5
aditya@Dell-Inspiron:~$ cat hw_ver
X1
```

- the `rootfs` file is the firmware's root file system
- now we can mount it to see the file system contents
```
aditya@Dell-Inspiron:~$ mkdir mnt
aditya@Dell-Inspiron:~$ sudo mount -o ro,loop rootfs mnt
```

```
aditya@Dell-Inspiron:~/mnt$ ls -la
total 4
drwxrwxr-x 14 root   root    257 Aug 10  2022 .
drwxr-x--- 16 aditya aditya 4096 Nov 29 14:20 ..
-rw-rw-r--  1 root   root      0 Aug 10  2022 .lstripped
drwxrwxr-x  3 root   root   3225 Aug 10  2022 bin
lrwxrwxrwx  1 root   root     13 Aug 10  2022 config -> ./var/config/
drwxrwxr-x  2 root   root   3091 Aug 10  2022 dev
drwxrwxr-x  7 root   root    926 Oct  1  2023 etc
drwxrwxr-x  3 root   root     31 Oct  1  2023 home
drwxrwxr-x  2 root   root      3 Oct  1  2023 image
drwxrwxr-x  6 root   root   2580 Aug 10  2022 lib
lrwxrwxrwx  1 root   root      8 Aug 10  2022 mnt -> /var/mnt
drwxrwxr-x  2 root   root      3 Aug 10  2022 overlay
drwxrwxr-x  2 root   root      3 Aug 10  2022 proc
drwxrwxr-x  2 root   root      3 Aug 10  2022 run
lrwxrwxrwx  1 root   root      4 Aug 10  2022 sbin -> /bin
drwxrwxr-x  2 root   root      3 Aug 10  2022 sys
lrwxrwxrwx  1 root   root      8 Aug 10  2022 tmp -> /var/tmp
drwxrwxr-x  3 root   root     28 Aug 10  2022 usr
drwxrwxr-x  2 root   root      3 Aug 10  2022 var
```

- a config file is symlinked to ./var/config/
- but apparently the /var directory is empty
- so I searched for config in the directory
<img width="1349" height="549" alt="image" src="https://github.com/user-attachments/assets/655dfe25-d820-4961-8072-20a8424a29f7" />

- opening the `config_default.xml` reveals a whole bunch of data about the firmware in written in XML
- I searched for `{` to check if there was anything resembling the flag and there was!
```
<Value Name="SUSER_NAME" Value="admin"/>
<Value Name="SUSER_PASSWORD" Value="HTB{N0w_Y0u_C4n_L0g1n}"/>
```

## Flag:

```
HTB{N0w_Y0u_C4n_L0g1n}
```

## Concepts Learnt: 
- mounting a firmware's filesystem using sudo mount
- exploring directories in linux
- revision about symlinks

## Notes:
- exploring /home directory gave some hope - "almost there"

***


# Speed Thrills But Kills
`desc.txt'
> i recently got involved in a hit and run case in pune, that kids porsche was going wayy too fast, if only i knew what the VIN of the car was :(


## Solution:

- open `trace_captured.sal` file Logic2
- Since car's OBD port communicates with a CAN signal we can use CAN analyser in Logic2
<img width="485" height="387" alt="image" src="https://github.com/user-attachments/assets/43705c97-5d79-45b6-997e-c7d70c154fdc" />


- The measurement gives a pulse width of approximately 8 µs. Hence the bitrate is 125000 bits/s
<img width="1919" height="1011" alt="image" src="https://github.com/user-attachments/assets/8995ec7e-ccd3-4d5a-9e4b-2fe867f80bab" />


- selecting `ASCII` mode give us this
<img width="1905" height="1019" alt="image" src="https://github.com/user-attachments/assets/76f66b93-daa5-4441-b4e3-73b2c50a2659" />

- Here we can directly see something that looks like the flag HTB{ ....
- exporting the file as CSV to clearly read the flag
<img width="687" height="678" alt="image" src="https://github.com/user-attachments/assets/cbac629c-777e-4db3-aa55-efadace73f09" />


- hence the flag can be read as HTB{v1n_c42_h4ck1n9_15_1337!*0^}


## Flag:
```
HTB{v1n_c42_h4ck1n9_15_1337!*0^}
```

## Concepts learnt:
- Car communicates it's data through OBD port with the CAN bus
- What is a CAN bus and other types of communication protocalls
- how to find the bitrate of a signal trace


## Notes:
I was correct when I initally analysed using CAN but didn't realise that I had to change the bitrate. I kept trying with other analysers and began my research about how it basically all works. Then I learnt how to find the bitrate and voila!

## Resources:
- (https://www.csselectronics.com/pages/obd2-explained-simple-intro)
- (https://walkertexaslawyer.com/your-car-probably-has-computer-data-on-your-accident/)
- (https://www.youtube.com/watch?v=IyGwvGzrqp8)
- (https://en.wikipedia.org/wiki/CAN_bus)
- (https://www.youtube.com/watch?v=9sas4uW4-Vg&t=808s)



 
