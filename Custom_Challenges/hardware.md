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

*** 

# Red Devil
`README.md`
> this is the worst football team ever(i dont even watch football lmeow)

## Solution: 

- the signal.cf32 file is a 32-bit floating-point data
- we need to decode and analyze the captured signal
- I used the `rtl-433` utility for this
- running `rtl_433 -A signal.cf32` gives us the output

```
- aditya@Dell-Inspiron:~/Custom_C/hardware/red-devil$ rtl_433 -A signal.cf32
rtl_433 version 21.12 (2021-12-14) inputs file rtl_tcp RTL-SDR SoapySDR
Use -h for usage help and see https://triq.org/ for documentation.
Trying conf file at "rtl_433.conf"...
Trying conf file at "/home/aditya/.config/rtl_433/rtl_433.conf"...
Trying conf file at "/usr/local/etc/rtl_433/rtl_433.conf"...
Trying conf file at "/etc/rtl_433/rtl_433.conf"...
Registered 176 out of 207 device decoding protocols [ 1-4 8 11-12 15-17 19-23 25-26 29-36 38-60 63 67-71 73-100 102-105 108-116 119 121 124-128 130-149 151-161 163-168 170-175 177-197 199 201-207 ]
Test mode active. Reading samples from file: signal.cf32
baseband_demod_FM_cs16: low pass filter for 250000 Hz at cutoff 25000 Hz, 40.0 us
Detected OOK package    @0.220228s
Analyzing pulses...
Total count:  185,  width: 1837.12 ms           (459281 S)
Pulse width distribution:
 [ 0] count:  114,  width: 3608 us [3604;3624]  ( 902 S)
 [ 1] count:   71,  width: 7204 us [7200;7208]  (1801 S)
Gap width distribution:
 [ 0] count:   71,  width: 7172 us [7172;7180]  (1793 S)
 [ 1] count:  113,  width: 3576 us [3576;3584]  ( 894 S)
Pulse period distribution:
 [ 0] count:   57,  width: 10784 us [10780;10796]       (2696 S)
 [ 1] count:   42,  width: 14380 us [14376;14384]       (3595 S)
 [ 2] count:   85,  width: 7188 us [7184;7196]  (1797 S)
Pulse timing distribution:
 [ 0] count:  227,  width: 3592 us [3576;3624]  ( 898 S)
 [ 1] count:  142,  width: 7188 us [7172;7208]  (1797 S)
 [ 2] count:    1,  width: 72084 us [72084;72084]       (18021 S)
Level estimates [high, low]:  15985,    488
RSSI: -0.2 dB SNR: 30.3 dB Noise: -30.5 dB
Frequency offsets [F1, F2]:   -5928,      0     (-22.6 kHz, +0.0 kHz)
Guessing modulation: Manchester coding
view at https://triq.org/pdv/#AAB1030E081C14FFFF819191919191919191919191919191918080808090818080918090808180918091808080919191808091808080918090808081908191918091809180809081809190808080819180918080808090819180809081808090819081919081809081808091908190808180809081908180919080808081809081808091908081809081919080808081908180809081809081808080808090818080808090819081808080918080809180918080809180918080809190808080819255
Attempting demodulation... short_width: 3608, long_width: 0, reset_limit: 7184, sync_width: 0
Use a flex decoder with -X 'n=name,m=OOK_MC_ZEROBIT,s=3608,l=0,r=7184'
pulse_demod_manchester_zerobit(): Analyzer Device
bitbuffer:: Number of rows: 1
[00] {256} 2a aa aa aa 0c 4e 48 54 42 7b 52 46 5f 48 34 63 6b 31 6e 36 5f 31 73 5f 63 30 30 6c 21 21 21 7d

```

- converting the hex to string gives
  
```
48 54 42 7b 52 46 5f 48 34 63 6b 31 6e 36 5f 31 73 5f 63 30 30 6c 21 21 21 7d
HTB{RF_H4ck1n6_1s_c00l!!!}
```

## flag:

```
HTB{RF_H4ck1n6_1s_c00l!!!}
```

## Concepts learnt:
- .cf32 files are radio frequency data in 32-bit complex floating-point.
- analysing these files involves software-defined radio (SDR) captures
  
## Resources:
- (https://www.youtube.com/watch?v=JdzVIjKA68o)
- (https://github.com/merbanan/rtl_433)

***

# I like logic more

## Solution:

- MicroSD cards communicate using the SPI protocol
- We need to identify which channel corresponds to which line of SPI

- Clock (channel 3): line with regular, rapid square wave pulses. These often appear in bursts (typically groups of 8) 

- Enable (channel 2): signal that stays in one state (usually High) and drops (Active Low) to "frame" the activity on the Clock and Data lines. It usually stays low for the duration of the data transfer.

- MISO / MOSI (Data): These lines will switch states (High/Low) in sync with the clock pulses.
- MISO: Chanel 0
- MOSI: Channel 1
<img width="1919" height="1019" alt="image" src="https://github.com/user-attachments/assets/52b1ee48-940f-4592-b1ca-3c41c395d1fc" />

- Now we can export the data in ASCII format into a CSV
<img width="440" height="708" alt="image" src="https://github.com/user-attachments/assets/ab6244cd-5013-47b2-97ec-50a872506751" />

- now we search for "H" hoping to find HTB{ i.e. the flag format
<img width="715" height="719" alt="image" src="https://github.com/user-attachments/assets/47abf233-218e-44e4-9726-628085ac6719" />


- Here we find the flag that is `HTB{unp2073c73d_532141_p2070c015_0n_53cu23_d3v1c35}`


## Flag:

```
HTB{unp2073c73d_532141_p2070c015_0n_53cu23_d3v1c35}
```

## Concepts learnt:

- SD cards communicate with SPI protocol
- How SPI signals can be interpretted from the channels
- Converting the output data to ASCII and exporting as CSV


## Resources:

- (https://electronics.stackexchange.com/questions/232885/how-does-an-sd-card-communicate-with-a-computer)
- (https://support.saleae.com/product/user-guide/protocol-analyzers/analyzer-user-guides/using-spi)
- (https://zeroalpha.com.au/services/data-recovery-blog/sd/sd-and-micro-sd-pinout-description-including-spi-protocol#:~:text=MicroSD%20SPI%20Protocol%20Pinout,is%20not%20used%20for%20SPI.)






 
