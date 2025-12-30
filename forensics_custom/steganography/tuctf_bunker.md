# TUCTF Bunker


## Solution:

- we see two files inside the zip `Bunker_DB` and `Bunker_DMP.kdbx`
- using file to check
```
┌──(kali㉿kali)-[~/Desktop/Bunker]
└─$ file Bunker_DB Bunker_DMP.kdbx
Bunker_DB:       Keepass password database 2.x KDBX
Bunker_DMP.kdbx: Mini DuMP crash report, 17 streams, Sun Nov 17 22:47:09 2024, 0x621826 type
```

- The challenge relies on a known vulnerability (likely CVE-2023-32784) where the KeePass master key can be recovered from a memory dump of the process
- extract the KeePass DB master key using the memory dump of the time when the password was entered
- https://github.com/vdohney/keepass-password-dumper
```                                                                                                                                                                                                                                       
┌──(kali㉿kali)-[~/Desktop/Bunker/keepass-password-dumper]
└─$ dotnet run /home/kali/Desktop/Bunker/Bunker_DMP.kdbx
```

<img width="781" height="259" alt="image" src="https://github.com/user-attachments/assets/41356703-d66c-46c5-bda3-aac294f81124" />

- assuming the key is most likely to be `gL0Ry_2_M4nk1Nd!_Y0RH4`
- now open the DB in keypassxc

<img width="1441" height="1107" alt="image" src="https://github.com/user-attachments/assets/e598685d-9d99-49a0-8462-88a1aa1bd15d" />


## Flag:

```
TUCTF{Th1s_C4nn0T_ConT1nu3}
```
